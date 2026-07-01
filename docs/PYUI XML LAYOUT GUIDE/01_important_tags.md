# Boleplate Layout and Important Tags

You can get started with a new layout by using this:

    <pyui>
    <metadata>
    <version>1.0.0</version>
    </metadata>
    <form-settings>
    <title>PyUI Portal - Hello World</title>
    </form-settings>
    <main-content default-active-window="boilderplateview">
    <window id="boilderplateview">
    
    </window>
    </main-content>
    </pyui>


1. **pyui:** All layout files(except components) must be wrapped inside the pyui tag
2. **version:** The current xml version
3. **form-settings:** this section contains the startup forms layout settings (discussed later).
4. **main-content:** This contains all windows/views of the form both active and inactive. the default-active-window is the window/view that is shown at start of the form
5. **window:** All components of a view/window is wrapped inside this tag. All windows must have a unique **id**.

## Important Tags
All tags present in html are not possible to be supported in v1.0. However you can easily add new tags with few changes in settings (Refer *Adding custom tags to compiler and maping them to converter*)

### Component Tags

#### 1. Text
This is used to write text in the window. Equivalent to *span* in html.

Attributes supported:

- **id:** For unique identification of the element
- **innerText:** The text to be displayed
- **style-class:** Equivalent to class attribute in html. This is to define all classes in a string.

Example:

    <Text id="example-text" style-class="text-style" innerText="Hello World"></Text>

#### 2. Button 

For adding button to the wndow. Equivalent to *button* in html.

Attributes supported:

- **id:** For unique identification of the element
- **innerText:** The text to be displayed
- **style-class:** Equivalent to class attribute in html. This is to define all classes in a string.

Example:

    <Button id="example-button" style-class="button-style" innerText="Click Here"></Button>

#### 3. Input

For adding input field to the window. Equivalent to *button* in html.

Attributes supported:

- **id:** For unique identification of the element
- **style-class:** Equivalent to class attribute in html. This is to define all classes in a string.
- **placeholder:** For the input placeholder
- **type:** The input type. Supports the html input types (text,password,options etc)

Example:

    <Input id="example-input" style-class="input-style" placeholder="input placeholder" type="text"></Input>

#### 4. Para (Paragraph tag)

For adding input field to the window. Eqivalent to *p* in html.

Attributes supported:

- **id:** For unique identification of the element
- **innerText:** The text to be displayed
- **style-class:** Equivalent to class attribute in html. This is to define all classes in a string.

Example: [Same as Text](#1-text)

#### 5. Br 

For making a space vertically between two components. Eqivalent to *br* in the html.

Example:

    <br></br>

#### 6. img (Image tag)

For adding image to window. Equivalent to *img* in the html.

Attributes supported:

- **id:** For unique identification of the element
- **style-class:** Equivalent to class attribute in html. This is to define all classes in a string.
- **src:** Source of image file.
- **alt:** Alternate title to show when source is not available.
- **width:** Width of the image 
- **height:** Height of the image

Example:

    <img id="image-id" style-class="img-style" src="resources/img.png" alt="Image can not be loaded"></img>

#### 7. video

For playing videos in window. Equivalent to *video* in html.

Attributes supported:

- **id:** For unique identification of the element
- **style-class:** Equivalent to class attribute in html. This is to define all classes in a string.
- **src:** Source of video file.
- **controls:** Controls for the video. See Html documentation for this attribute.
- **autoplay:** To autoplay on window load.
- **loop:** To play the video to loop.
- **muted:** To keep video 
- **width:** Width of the image 
- **height:** Height of the image
- **poster:** Image path of the poster to be shown.

Example:

    <video id="video-id" style-class="video-style" src="resources/video.mp4"></video>

**Very less tags compatitable? Wait you can very easily extend tags to support more html tags.Refer:** [Extending Tags for PYUI](#)


### Window Adjustment Tags

#### 1. style 

For adding a external style file to the layout xml.

Atributes supported:

- **file:** file of the style file.**file sshould be strictly inside** *layouts/styles* **folder**

**Note:** The style tag is only valid outside **main-content** tag and must be a direct child to **pyui** tag.It will be ignored by compiler if it is put inside the **main-content** tag. It will cause error if put inside the **form-settings** or any other tag. 

#### 2. main-content

This is the single holder for all windows inside the form.

Attributes supported:

- **default-active-window:** To set a default active window for the form. It takes the id of the window

Example:

    <main-content default-active-window="window1">

    .....

    <window id="window1*>

    </window>

    .....

    <main-content>

#### 3. window:

It is a single view point of a form. One can change them easily using PYUI apis.

Attributes supported:

- **id:** Id of the window.
- **style-class:** Equivalent to class attribute in html. This is to define all classes in a string.


### Form Adjustement Tags

This tags are defined in this format 

    <tag>Value</tag>

#### 1. metadata

To store version of app used. In future builds it will support more tags. However you can even [extend this using settings.py](#)

- **version:** App version

Example:

    <metadata>

        <version>1.0.0</version>

    </metadata>


#### 2. Form settings

    - Supports almost all Pywebview window settings.

Few attributes:

- **resizable:** Is the page resizeable.
- **title:** Title of the form.
- **width:** width of the page.
- **height:** height of the page.

See [Pywebview window api](https://pywebview.flowrl.com/api/) for all the settings.

Example:

    <form-settings>
    <title>PyUI Portal - Hello World</title>
    <resizeable>False</resizeable>
    </form-settings>



