# Other functions

This page contains the other important functions in PYUI Class

## Changing Window

To change a window (switch) you need to use `changeWindow()` function.

#### Example

    self.pyui.changeWindow("window-name")

## Get a PYUI Raw Node (PyUILayoutNode)

Get the PyUILayoutNode using `PyUILayoutNode()` function.

#### Example

    self.pyui.PyUILayoutNode('id-of-element')

## Open a new PYUI form

To open a same or another form dynamically use `loadForm()` function.

#### Example

    self.pyui.loadForm('form-name')

**Note:** if form is is sub-folder for example `example/form.xml` then load form will take `example.form` as the `form-name`.Basically replace `/` with `.`.

## Get Pywebview window

`PYUI 0.6` started exposing raw `Pywebview` webview window. you can get it using `self.pyui.window`

#### Example

    self.pyui.window # <- returns current instance of webview window


## End the current form

To end the current form use `End()`

#### Example

    self.pyui.End()