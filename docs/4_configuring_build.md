# Configuring Build Pipeline

`PYUI` provides build time `Hook` and start and end time `Co-rountines` to add features to the `PYUI` build pipeline. It also privides few other features that can be controlled using `settings.py`

Following page lists all capabilities of `settings.py`

## Configuring Tailwind

`PYUI` is tailwind compatitable so enable tailwind you need to change the `TAILWIND_ENABLED` flag to true in `settings.CompilerSettings`

    TAILWIND_ENABLED = True # <-- flag changed to true

See `configuring tailwind` page for more details for configuring tailwind


## Setting Max Build Temporary Limit

`PYUI` compiles and stores the required files in `build` folder inside your project directory. `ISSUE_TEMP_OVERCROWDING_LIMIT` sets limit to hoe many preivous build to store before flushing all of them.

     ISSUE_TEMP_OVERCROWDING_LIMIT = 10 # <-- by default set to 10

**Note:** Setting this value to 0 you will be triggered warning each time you do a new build to ask you for cleaning so better set it as number > 3 atleast.

## Sources

For storing the other `assets` for the app the `sources` folder is used however you can change the folder name and rename it in `settings.py`

    SOURCE_FOLDER = 'sources' # <-- change this if needed

## Hooks

`Hooks` allow to insert your python function before the several build steps.

     HOOK_MAP = {

    "COMPILATION":None,
    "CONVERTION":None,
    "TAILWIND_STYLE_COMPILATION":None,
    "STYLE_COPY":None,
    "PACKAGE_COPY":None,
    "JS_COPY":None,
    "CODE_COPY":None,
    "BUILD_START":None
    }

`None` by default means no function to call before starting particular build feature. We can configure to add functions for example

    def random_fun(build_folder):

        print("Simple call.....")
        print("Build folder:",build_folder)

    HOOK_MAP = {

        ........

        "STYLE_COPY":random_fun,

        ........
    }

So before style copy happens your function will be called. You can make a style compiler of your own and inject it here to make your styles before copy happens (just a scenario).

## Pre and Post Co-routine

`PYUI` exposes two classes `PreExecuteCoRountine` and `PostExecuteCoRountine` this are initated at the very moment build starts. They have a function `entry`. For `PreExecuteCoRountine` it is called at extreme start of the build and for `PostExecuteCoRountine` is called at extreme end of thr build.


    class PreExecuteCoRountine:
        def __init__(self):
            '''
            This section will be initated at very starting of the compilation chain
            '''
            pass
        def entry(self):
            '''
            Add the code to be ran at extreme start of compilation pipeline here
            '''
            pass

and 

    class PostExecuteCoRoutine:
    
        def __init__(self):
            '''
            This section will be initated at very starting of the compilation chain
            '''
            pass

        def entry(self):
            '''
            Add the code to be ran at extreme end of compilation pipeline here
            '''
            pass