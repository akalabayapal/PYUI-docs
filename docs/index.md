## Getting Started
Welcome to the guide for **PYUI**. This and following pages will take you through:

 - Installation
 - Starting off with a boilerplate project
 - Project Structure explanation
 - PYUI Build Tools
 - PYUI XML rules and Tags
 - PYUI API reference
 -  Advance features (Python Hooks, Data streaming, Security, Auto Update and more)
 - Debugging and error catching by exploring temporary build folders
 - Adding custom tags and Syscalls and registering custom JS bundles
 - Customizing build pipeline 
 - Bundling chromium with project (configuration and downsides)

## Architecture Philosophy
 This framework uses the elegance of webview (provided by **Pywebview**) and using strict xml layout for designing pages and python code for logic. This framework compiles the layouts into 2 parts:
 
 - BIN file (This contains a compiled tree used for optimised object acess)
 - .html your strict xml gets compiled to pure safe html to be displayed
 
By doing this one can produce elegant looking apps with size very less compared to huge frameworks like PYQT yet maintain the modern layouts as it supports css and frameworks like tailwind or bootstrap and more via configuring project properly.
We will touch internals of framework later and in debugging section more.

## Installation
 1.You must have python with pip installed in your system right now PYUI is tested in windows and linux only.
 `pip install pyui-desktop`
 
Thats it, it is all you need for installing PYUI

## Starting off with a boilerplate project

    mkdir HelloWorldApp
    python -m PYUI.create HelloWorldApp
    
Thats it, go to the directory you mentioned 

    cd HelloWorldApp


Now Compile for first time

    python -m PYUI.buildtools --compileexe ./  --target DEBUG --name HelloWorldApp
  
  Go to the DEBUG directory made and execute
  
    HelloWorldApp.exe

**Note:** Replace 'HelloWorldApp' with the name of your project.


**Result:**
![enter image description here](assets/hello.png)