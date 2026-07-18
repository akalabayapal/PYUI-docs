# Hooks and Pipelines

`PYUI` has two very important feature `Hooks` and `Pipeline`. Pipeline helps in sending a batched set of `DOM Manipulation` commands to UI.While Hooks are lossy batched channels for sending data at a fast fixed rate to UI. In this page we will discuss both of them in more detail.


## Pipeline

For example you need to update 10 things in UI. Normally you will call dedicated functions for the changes 10 time. But `PYUI` has much better solution for this type of senarious.Rather than sending 10 different `message` tokens to `JS`.Pipeline gives a way to batch several such `DOM Manipulation` messages into one and send them at onces. This reduces the networking overhead significantly.


#### Example

Before we give the general syntax for `Pipeline` it is better to discuss it via a example, otherwise syntax may cause several confusions.

    from PYUI.Package.PYUI import Pipeline # this import is needed

    class App:
        
        def __init__(self,obj:PYUI):
            self.pyui = obj

            self.p = Pipeline()

            # build the pipeline for sending batch
            self.p.add(self.pyui.setText)
            self.p.add(self.pyui.setStyle)
            self.p.add(self.pyui.setStyle)
            self.p.add(self.pyui.changeClass)

            example_send() # call the function for batch update


        def example_send(self):

            # send batched UI mutation commands

            self.p.call(
                [
                    ('eid1','New text') # params for calling 1st function (setText)
                    ('eid2','color','red') # params for calling 2nd function (setStyle)
                    ('eid1','backgroundColor','blue') # params for calling 3rd function (setStyle)
                    ('eid2','class_new','ADD') # params for calling 4th function (changeClass)
                ]
            )


So the steps to use `Pipeline` are:

1. Make a instance of `Pipeline`

        p = Pipeline()

2. Add the `PYUI` functions that are **compatible with Pipeline**

    Look for the functions in `PYUI Libary` that have `ret` parameter are directly compatible with `Pipeline`.

        p.add(self.pyui.function)
 
3. When needed call pyui with **list of tuple** of parameters that needed to be passed to the functions

        p.call(
            [
                (parms_of_func1),
                (params_of_func2)
                .
                .
                .
                (params_of_func_n)
            ]
        )

### When to use Pipeline

Whenever there is a need for sending a set of pre-known UI mutations it is better to use `Pipeline`. Also always define a pipeline ones and use it multiple times.

#### Bad usage

    def example_send(self):

            p = Pipeline()

            # build the pipeline for sending batch
            p.add(self.pyui.setText)
            p.add(self.pyui.setStyle)
            p.add(self.pyui.setStyle)
            p.add(self.pyui.changeClass)

            # send batched UI mutation commands

            p.call(
                [
                    ('eid1','New text') # params for calling 1st function (setText)
                    ('eid2','color','red') # params for calling 2nd function (setStyle)
                    ('eid1','backgroundColor','blue') # params for calling 3rd function (setStyle)
                    ('eid2','class_new','ADD') # params for calling 4th function (changeClass)
                ]
            )

This will make `Pipeline` instance each time `example_send` is called. So better usage is to define `Pipeline` in a shared space like in `__init__` functions and only use `call` inside mutating functions.

### Arithmetic approach of adding targets to Pipeline

Using `p.add()` multiple time clutter the code. So `PYUI 0.6` intoduced arithmetic approach of adding targets to pipeline

#### Example 

    p = Pipeline() + self.pyui.setText + self.pyui.setStyle + self.pyui.setStyle + self.pyui.changeClass

Is equivalent to:

    p = Pipeline()

    # build the pipeline for sending batch
    p.add(self.pyui.setText)
    p.add(self.pyui.setStyle)
    p.add(self.pyui.setStyle)
    p.add(self.pyui.changeClass)

Using this approach will keep your code clean and crisp

## Hooks

When a batch instruction (i.e Pipeline) is to be called in a fixed interval multiple times using `Hooks` is better. But `Hooks` serve much more than just that:

1. **FPS Control:** Your updates might be very fast but you can control how fast (in fps) you want that to be updated in your UI. Your sensor might give results in 100 fps.  But you might want to send the changes to UI at only 30 fps you can use `Hooks`.

2. **Lossy updates:** For fast updates it is needed to preserve each updates. So some updates can be dropped from being shown from UI.This improves application response

3. **Higher Priority:** Hook messages travel in much higher priority than normal updates. Hence the UI updates remain more clean.

4. **Update on demand:** The updates are only send if there is any change. For example a progressbar connected to hook the percentage to change the progress will be sent only when there is change in progress than sending it at high fps.


#### Example

    from PYUI.Package.PYUI import Pipeline,Hook # this import is needed
    import time

    class App:
        
        def __init__(self,obj:PYUI):
            self.pyui = obj

            self.p = Pipeline()

            # build the pipeline for sending batch
            self.p.add(self.pyui.setText)
            self.p.add(self.pyui.setStyle)
            self.p.add(self.pyui.setStyle)
            self.p.add(self.pyui.changeClass)

            self.h = Hook(pipe=self.p,pyui=self.pyui,fps=15)

            # simulating fast updates 

            while True:
                example_send() # call the function for batch update
                time.sleep(0.01) # small pause


        def example_send(self):

            # send updates

            self.h.update(
                [
                    ('eid1',str(time.time_ns())) # params for calling 1st function (setText)
                    ('eid2','color','red') # params for calling 2nd function (setStyle)
                    ('eid1','backgroundColor','blue') # params for calling 3rd function (setStyle)
                    ('eid2','class_new','ADD') # params for calling 4th function (changeClass)
                ]
            )


Eventhough updates are coming at **100fps** the updates happen only at **15fps** only. 

Steps to use `Hook`:

1. Make a Pipeline and add the functions

2. Call `.update` function to update the values

