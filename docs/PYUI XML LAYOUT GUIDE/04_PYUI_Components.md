# Reusing Layouts using Components

PYUI supports static component reusage and props system. All your components must lie inside `layouts\components` folder.

## Creating a component
Following is the format of defining a Component with `prop` as one of the properly.To acess properties passed use `{property_name_passed}`

    <ComponentFile>
    <container>
        <Text id="text_component" innerText="{prop}"></Text>
    </container>
    </ComponentFile>

## Default property support.

`PYUI 1.0.0` added defualt property support.Now you can assign a default value to props and those will be used when explicitly properties are not passed from the parent layout, or from the python dynamic component handler.

    <ComponentFile prop="default_content">
    <container>
        <Text id="text_component" innerText="{prop}"></Text>
    </container>
    </ComponentFile>

We need to define the default props as attribute of the `ComponentFile` tag as shown above.

**Note:** Keeping components empty are strictly blocked and atleast one tag should be a child of ComponentFile (like here it is container)

## Acessing component from a layout file

For using a component from the layout file. Use the following

    <Component file="ex.xml" name="ex" prop="Sample Text"></Component>

1. **file:** This is the file for components.This file path is relative to `components` folder.
2. **name:** This is used as base name for all the id present in the component. This is done to avoid the ID Collision when same component is used twice or more.Do not use same name for any two components from same file
3. **prop:** A example property passed to the component.Infact you can pass as many you want

        <Component file="ex.xml" name="person_1" name="John" age="24" height="169cm"></Component>
        <Component file="ex.xml" name="person_2" name="Harry" age="44" height="182cm"></Component>
        ..............

## Acessing ID from PYUI

The ID are changed by adding the base `name` provided to keep uniqueness for each component.To get or set a component element the id is changed from `id` to `name_id`

For example consider this component again:

        <ComponentFile>
        <container>
            <Text id="text_component" innerText="{prop}"></Text>
        </container>
        </ComponentFile>

Now if we call the component from a layout file by:

        <Component file="ex.xml" name="ex" prop="Sample Text"></Component>

The id of `Text` will be changed from `text_component` to `ex_text_component` internally during compilation.

### Important points to Remember

1. The `name` is used as a base id for the components hence all id gets renamed to `name_actual_id_given` by the compiler internally.

2. If a id is kept blank (i.e ` id="" `) then it will be assigned a id same as the `name` of the component.

### Acessing Component's inner elements from PYTHON Runtime


    from PYUI.Package.PYUI import cpath

    .....

    component_id =cpath('exampleComponent','txt') # <- cpath is a function from PYUI.Package.PYUI helps to write name mangling cleanly

    e = Element(component_id,self.pyui) # <- self.pyui is just the pyui instance

    .....

**Note:** `PYUI 1.0.0` added `cpath` function. Rather than writing the mangled component names on your own use `cpath(component_name,component_element_id)` internally cpath with return the mangled id.