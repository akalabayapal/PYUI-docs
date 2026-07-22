# PYUI Sources

`PYUI` project has a `sources` folder. In this folder you can drop sources that you want to be compiled with your project in the standalone executable when you build `executable`.


## An Optimisation

As a developer you might build the project temporarily 100s of times so copying `sources` too many times to the `temp_XYZ` build folder will slow down the build procedure and eat up disk space. To battle with this problem `PYUI` uses dynamic path resolving, when you are hotreloading a layout or compiled and run it the path is resolved to the sources directory directly without copying. But when you actually compile it to exe then it is copied and compiled into standalone executable.

## Understanding Paths

### XML Layout 

There are 3 types of url you can add to src properties of img,video or etc.

#### Accessing urls

If you want to add urls from a server to the layout by url directly use the link

Example:

    <img src="https://www.google.com/image/xyz"></img>

#### Accessing local paths

If you want to access the local paths in your computer you can use `local@` before your url to define it.

Example:

    <img src="local@C://images/img.jpg"></img>

#### Accessing from source folder

If you have a resource in source folder you can use `src@` before path relative to asset with respect to `sources` folder.

Example:

    <img src="src@asset/avatar.jpg"></img>

### Python Side

To set src property from python side the `uri` you are using needs to be taken care.

#### Local file

To add a local file use the same `local@your_path` from python to set that path as resource

#### Url from internet

Just put the url as it is in the src property to set it as the resource

#### From source folder

1. Get the source path for resource

    path = self.pyui.src_path("asset/avatar.jpg") 

2. Set it to src 

    e = Element('profile')
    e.attrib.src = path

**Note:**`src_path` dynamically returns correct asset path both in compiled state and in normal state. So whenever you are accessing a source file do it via `src_path`

