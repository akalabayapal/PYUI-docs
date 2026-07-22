# Introduction to PYUI Python Interface

PYUI comes with `PYUI` standard libary. It provides the functionalities for:

1. DOM Manipulation
    - Using Element Instance 
    - Using proceudral Legacy `PYUI` calls
2. Event callbacks setting , resetting and stopping
3. Thread Management Model ( `group`, `@background` , `commit`)
4. Hooks and Pipelines
5. Registering and Unregistering `Custom Syscalls` to execute other JS functions
6. Sending `Custom Syscalls` and handling there callbacks from JS and Python side.
7. Adding and Removing Components dynamically from Python Runtime
8. Other functions

## Architecture Summary

Before jumping into the libary it is important to understand a bit of how `PYUI` works under the hood.

### The structure of Message

As PYUI works using sending messages from PYTHON to JS and vice versa all messages share a common structure. Example

    {'uuid':'','type':'','other_data':'' .... , 'sign':''}

**uuid:** It is in a unique identifier to identify a singular message. It is also used to make callback tables and dispatch callbacks to correct queue.

**type:** This stores the `SYSCALL` code (see below). To identify what to do and what instruction to execute in both JS and Python side.

**other_data:** This contains other data specific to the `SYSCALL` type.

**sign:** It contains a `HMAC` sign for the whole message JSON leaving the sign key itself. Helps in verifiying the authenticity of the message.

### One important termology: SYSCALLS

All message that are going from `PYTHON` to `JS` or `JS` to `PYTHON` contains a Message code that is `SYSCALL` this helps both runtime understand what to do.

For example `EXECUTE_JS` syscall is used to execute some `JS` script. This is sent from `Python` to `JS` runtime with required data about what to execute. JS unpacks the `JSON` reads the syscall and sends it to the `handleExecuteJS` function that reads the `TYPE` from the same JSON and it can be `REGISTER_CALLBACK` `SET_...` etc.. and performs the event.


### How Python to JS Communication happens

1. Once you set a property in Python side. Your request is packaged into a JSON Procedure call like shown above.
2. The `PYUI` libary using a threadsafe queue `SEND_QUEUE` dispatches this call to the `websocket` thread.
3. Websocket thread `get` from the queue continuously and sends it to `JS` client.
4. `JS` side recives the message. Parses the `JSON` and sends it to a `handleMessage` function.
5. `handleMessage` unpacks the `JSON` and finds out the `SYSCALL` and executes required information.


### How JS to Python Communication Happens

1. Similarly `JSON` is sent from `JS` to `Python` side.
2. `Python's` websocket thread captures it and reads the `uuid` and `JS SYSCALL TYPE`.
3. It selects the correct queue to dispatch the message to using the uuid (The uuid used while sending data from Python to JS side for requesting a callback or data is sent back), and dispatches it.
4. The thread (in case of callback) or the user thread (in case for getting DOM data) waits for the message using `queue.get()` once it recives the message.
    - In case of `Callbacks` the acutal callback function is executed with the `e.detail`(sent from JS side)
    - In case of `DOM get` the value is returned to user/developer. Developer then can use it in the program.

### Final words

This were the important things required for clear understanding of `PYUI Liabary` from next page we will cover the important `libary functionalites`.