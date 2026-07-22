# Syscalls

`PYUI` is a hybrid framework thst uses `JS` and `Python`. To eshtablish a proper communication protocol between both the runtimes I have made `Syscalls`. This are core to all messages sent from `JS` to `Python` or from `Python` to `JS`. Howerver `PYUI` support developers to register there own `Syscalls` and add functionallity to `PYUI`. In this page we will talk about `Custom Syscall`

## Why Custom Syscall ?

`Custom Syscalls` help developer to communicate to and fro between `JS` and `Python` runtime, without touching the websocket directly.`Custom Syscalls` were added to keep developers abstracted from touching sockets, and to enforce the proper messenging format. Developers not touching the syscalls explicitly keeps the message JSON coherent and interpretable in both `Python` or `JS` side.

## JS File in PYUI

Before we tell how to make our first syscall it is important to know how to make js file in `PYUI`. 

1. Go to `layouts/JS` folder
2. Make the js file of exacly same name as your form name with `.js` extension. For example if your form is `profile.xml` so make `profile.js`. Also it supports nested folders. Like if you have form in `auth/profile.xml` you have to make js file in `auth/profile.js`

## Making a custom syscall

Let us see the following example

#### Example

    // JS side
    function add(message)
    {
        // The message contains several things .msg is the actual content sent by Python
        const token = JSON.parse(message.msg);

        let a = token.a; 
        let b = token.b;

        const r = {"result":a+b};

        // send back result to Python
        sendSyscall('ADD',r);
        
    }

    bind('ADD',add); // <= bind the function to syscall 

and then in Python side

    // Python side

    // Register the syscall to Python PYUi
    self.pyui.RegisterSyscall('ADD')

    // register the callback for the syscall this is triggered when JS event come
    self.pyui.RegisterSyscallCallback('ADD',self.callback_result)

    // send callback
    toSend = {"a":10,"b":20}
    self.pyui.sendSyscall('ADD',toSend)


    def callback_result(self,pyui_instance,message):

        print(message) 

        # Expected: {"result":30}

1. We bind a function in JS side using `bind()` function.
2. We can use `sendSyscall()` function in JS side to send syscall.

        sendSyscall('SYSCALL_NAME',function)

3. In Python side we need to first register the syscall.

        self.pyui.RegisterSyscall('SYSCALL_NAME')

4. Then add a callback to it if you exepect some return response from JS side

        self.pyui.RegisterSyscallCallback('SYSCALL_NAME',func)

        # use args= to supply arguments

5. Send the syscall to JS whenever needed

        self.pyui.sendSyscall('SYSCALL_NAME',message)

        # message in string or json serialzeable objects

6. Make the callback function to accept the value from JS side

        def func(self,pyui_instance,message):
            .....

## Unregister a Syscall Callback or Remove a Syscall

### Unregister a Syscall callback

        self.pyui.UnSyscallRegisterCallBack("syscall_name")

### Remove a syscall

        self.pyui.RemoveSyscall('syscall_name')




