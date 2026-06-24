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
- **style-class:** For adding css class to the element
- **innerText:** The text to be displayed

Example:

    <Text id="example-text" style-class="text-style" innerText="Hello World"></Text>

#### 2. Button 
- **id:** For unique identification of the element
- **style-class:** For adding css class to the element
- **innerText:** The text to be displayed

Example:

    <Button id="example-button" style-class="button-style" innerText="Click Here"></Button>



### Window Adjustment Tags

### Form Adjustement Tags