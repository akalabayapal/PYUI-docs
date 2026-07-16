# DOM Event Handling, Callbacks and Threading model

`PYUI` supports adding event-callbacks to UI elements. There are two ways to do it we will discuss them one by one. Then focus on possible race conditions and how PYUI provides support to protect against potential race-coditions.

## Registering a Callback to a UI Event

### Using Element Class

1. First make instance of the element using `Element` class.

        e = Element('element_id',self.pyui)

2. Register the event

        e.on('event',self.callback,args)

    **event:** This is the event you want to register. See JS documentation so see supported EventListeners. `PYUI` is almost compatible with all the event listeners of JS.

    **callback:** The callback function you want to execute when the event happens.

    **args:** Extra arguments to be passed to the callback when it is called

3. Write the callback function

        def callback(self,o,m):

            print("Clicked")
            print("e.details message:",m)

**Note:** `args` parameter is optional

#### Here the full example of a event registration

    .....

    class App:

        def __init__(self,obj):

            self.pyui = obj

            e = Element('btn-example',self.pyui)

            e.on('click',self.btn_onclick,args=('This is argument passed','Another arg'))

        def btn_onclick(self,o,m,arg1,arg2):

            print("Clicked")
            print("Event details:",m)
            print("Extra args passed:",arg1,arg2)

    .....

    def entry(obj:PYUI):

        App(obj)

### Using Procedural API

Only the event registration step changes to:

    self.pyui.RegisterCallback('id','event',callback,args)

**Note:** If event was previously attached to another or same callback it will raise `CallbackCollisionError`.At a time only one callback must be registered to a unique event for a given ID.

**Note:** `args` parameter is optional
## Unregistering a Callback from a UI Event

### Using Element Class

Use the same `Element` instance or make a new one

    e.off('event')

    e.off('click') # The click event will be unregistered from the `Element`

### Using Procedural API

    self.pyui.UnRegisterCallback('id','event_name')

    # Example

    self.pyui.UnRegisterCallback('btn-example','click')

**Note:** UnRegistering a event that never existed will raise `CallbackNotFoundError`

## Re-Registering a new or existing Event

Re-registering event callback is only possible directly via Procedural API

    self.pyui.ReRegisterCallback('id','event_callback',callback,args)

**Note:** If the event was never registered before it will raise a `CallbackNotFoundError`

## Threading Model

To prevent developers from race conditions `PYUI` provides few solutions and protections.They are:

- `group` function
- `@background` decorator
- `commit()` function

### Group Function

When a callback is registered a new thread is launched and it handles all callback event for a particular id and a event. Sometimes it is important that two callbacks to run only sequencially so that there code sections do not interleave to protect against the race conditions.
Even though you never use group functions your callbacks will still be executed sequencially as `PYUI` by default groups all the callback threads into one.

But if you want a set of threads to execute concurrently without waiting for any other callbacks you can self group it

Example:

    group(callback_func,callback_func) 

Also if two callback functions need to mutate a shared variable but you don't want them to be grouped with all other global callbacks you can group them together in a separate group

Example:

    group(callback_1,callback_2)

#### Grouping multiple callbacks

Grouping is not restricted to only 2 callbacks you can group callbacks as much as you want like:

    group(a,b)
    group(b,c) 


This puts all a,b,c into a single group that means if at a time all a,b,c events or any two of the events needs to be executed they will be executed sequencially not concurrently.

### @background decorator

Now a problem comes that is if `PYUI` is sequencial in terms of callback there might be some long running callbacks that may stall the whole working of the Application. The solution given by PYUI is to use this decorator, adding this decorator to your function makes your function run concurrently and not sequencially.

Usage of this decorator is not limited only to callbacks they can be used in general purposes. Following examples illustrate them:

#### Example of @background in general purpose

Without @background 

    import time
    .....

    class App:

        def __init__(self,obj):

            self.pyui = obj

            self.task()
            print("Task started")
            
        
        def task(self):
            time.sleep(10)
            print("Long running task completed")
            

    .....

    def entry(obj:PYUI):

        App(obj)


Expected output:

    Long running task completed
    Task started 

With background

    import time
    .....

    class App:

        def __init__(self,obj):

            self.pyui = obj

            self.task()
            print("Task started")
            
        @background # <- background tag added
        def task(self):
            time.sleep(10)
            print("Long running task completed")
            

    .....

    def entry(obj:PYUI):

        App(obj)

Expected output:

    Task started
    Long running task completed

#### Example of @background in callback

We recomend to perform the critical section work inside the sequential callback and then offload the heavy tasks if needed to background worker.

    import time
    ....

    class App:

        def __init__(self,obj):

            self.pyui = obj

            e = Element('btn-example',self.pyui)
            self.global = 0

            e.on('click',self.btn_onclick,args=('This is argument passed','Another arg'))

        def btn_onclick(self,o,m,arg1,arg2):

            # doing critical section update
            self.global += 1
            #now offloading heavy tasks to background

            self.update_profile_task()

        @background
        def update_profile_task(self):

            #simulating network latency
            time.sleep(10)

            print("Update completed")

    .....

    def entry(obj:PYUI):

        App(obj)

### Commit function

Now as `@background` workers are concurrent they while writing back to set of shared variables may cause `variable tearing` leading to invalid state. This is a classic race-condition problem. Before we understand what commit function is it important to understand the concept of variable tearing

#### Variable Tearing

Variable tearing occurs when multiple related variables represent a single logical state but are updated separately. If another thread reads them during the update, it may observe a **partially updated (invalid) state**.

**Example:**

Initial state:

```text
x = 10
y = 20
```

Thread A updates both values:

```python
x = 30
# Context switch
y = 40
```

Before the second assignment completes, Thread B reads:

```text
x = 30
y = 20
```

Although both values are eventually updated, the combination `(30, 20)` was never a valid state—it exists only because the read occurred midway through the update.

#### How commit() solves it

`commit()` is a function under the `PYUI` class that ensures atomic updates in between **all background workers that are started from same group**. So grouping is a critical decision to stop problems like this.


API reference:

    self.pyui.commit(scope,**variable_name=value)

**scope:** Scope where your variable is present
**variable_name:** you can commit one or more variables at once for which this is infact designed

Example:

    self.pyui.commit(
        global,
        a = 20,
        b = 30,
        c = 40
    )

Usage:

        import time
        import random
    ....

    class App:

        def __init__(self,obj):

            self.pyui = obj

            e = Element('btn-example',self.pyui)
            self.global = 0

            #records 
            self.name = None
            self.author = None
            self.price = None

            e.on('click',self.btn_onclick)

        def btn_onclick(self,o,m):

            # doing critical section update
            self.global += 1
            #now offloading heavy tasks to background

            self.update_profile_task()

            # Random delay to simulate unpredictable thread scheduling
            time.sleep(5+random.random()*10)

            self.reader()

        @background
        def reader(self):
            print(self.name,self.author,self.price)


        @background
        def update_profile_task(self):

            #simulating network latency
            time.sleep(10)


            self.pyui.commit(
                self,
                name = 'Example book',
                author = 'akalabaya_pal',
                price = 100
            )
            print("Update completed")

    .....

    def entry(obj:PYUI):

        App(obj)


Using commit will ensure that you read either all `name`,`author` and `price` as `None` or the value set afterwards not mixed between two states.

