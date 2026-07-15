# Handling DOM Manipulation using PYUI

PYUI provides functionality for setting or getting DOM `attributes` and `properties`. This can be achived in multiple ways and each of them has there importance. We will cover them one by one.


## Understanding Boilerplate

    from PYUI.Package.PYUI import PYUI,Element,Component,cpath

    '''
    Element: OPP facade over procedural PYUI
    Component: Add and remove dynamic components from UI
    cpath: helps is easy resolving component ids you need not write "component_id_element_id" better use cpath(component_id,    element_id)
    both are equivalent.
    '''

    class App:

        def __init__(self,obj:PYUI):

            '''
            Entry point of App class.
            Use this class to work with index.xml
            '''
            self.pyui = obj


    def entry(obj:PYUI):

        """
        This is the entry point for your form index.xml
        You can write procedural code using this entrypoint by deleting OPP function. Or use the boilerplate App class
        You can rename the Class to anything
        """
        App(obj) # executing the app class

After initiating the environment. `bootstrap.py` loads your python code file and executes `entry` function. Prior to `PYUI 0.6` the PYUI mostly gave you a functional boilerplate. But from `PYUI 0.6` onwards it is recomended to use a `OOP` based strucure.So `entry` functio calls the `App` class. It is recomended to write code using this class.

## Using standard Element Class

The OOP Element class provides easy way to get and set DOM properties.

### style and attributes

`.style` and `.attrib` are two namespaces inside the `Element` class this helps to set and get `DOM properties and attributes`.

1. **.style:** This is used to get and set styles for elements. Use cammel case for hypened properties.
2. **.atrrib:** This is used to get and set both `attributes` and `properties` for a element in PYUI.

**Note:** If you are completely new to HTML attributes, properties and styles please refer to a standard HTML 


### Initiating by using a Element class

    e = Element(id,self.pyui) 

This makes a instance for the given `id`. Using this instance you can perform all kinds of dom manipulation

### Common DOM manipulation api reference and example

1. **Changing innertext for a component**
    
        e.text = 'new_text'

2. **Getting a particular id's attribute**

        val = e.attrib.attribute # This keep the attribute by reference, changing the variable `var` changes the attribute
        
        copy = str(e.attrib.attribute) # This keeps a copy of the attribute.You can typecast to the type of need

        hidden = bool(e.attrib.hidden) # Here hidden is a boolean hence we typecast to bool and store the copy

3. **Setting a particular id's attribute**

        e.attrib.attribute = value_of_attribute

        e.attrib.hidden = False # <- Hides the element completely (Example)

4. **Set a new Style for a id**

        e.style.style_attribute = 'new_style_value'

        e.style.color = 'red' 
        e.style.backgroundColor = 'blue' # <- Use camelCase for the properties with hypen in betweeen like background-color
    
5. **Add or Remove a Class**

        e.add_class('class_name')
        e.remove_class('class_name')
        e.toggle_class('class_name')


#### Dynamic attribute/style changing
There is also another way you can achive the above.Always the name of attribute or style you will change may not be known ahead of time. Use the functional approach to work

1. For getting a style

        e = Element('id')
        e.get_style('style-attribute') 

2. For setting style

        e.set_style('style-attribute','style-value')

3. For getting a attribute

        e.get_attribute('attribute-name')

4. For setting a attribute

        e.set_attribute('attribute-name','attribute-value')

## Using Procedural PYUI API calls

Prior to `PYUI 0.6`, PYUI had only procedural functions to handle DOM getting and setting. Although using `Element` class is much better in almost all cases using Procedural calls poses less overhead, helps better in debuging and might me useful in some cases.

1. Getting a attribute/property

        self.pyui.getAttrib('id','attribute-name')

        # Example

        self.pyui.getAttrib('input-id','value')

2. Getting tag of the Element

        self.pyui.getTag('id')

3. Setting attribute/property to DOM

        self.pyui.set('id','attribute','value')

        # Example

        self.pyui.set('input-id','value','new value')

4. Setting style for a Element

        self.pyui.setStyle('id','style_attribute','style_value')

        # Example

        self.pyui.setStyle('id','color','red')
        self.pyui.setStyle('id','backgroundColor','blue')

5. Getting style for a Element

        self.pyui.getStyle('id','style_attribute')

        # Example

        self.pyui.getStyle("id","color")

6. Changing Class

        # Adding Class

        self.pyui.changeClass('id','className','ADD')
        
        # Remove Class

        self.pyui.changeClass('id','className','REMOVE')

        # Toggle Class

        self.pyui.changeClass('id','className','TOGGLE')

7. Setting InnerText of a Element

        self.pyui.setText('id','new-text')

## Tradeoff's and when to use what

Eventhough usage of API is subjective. Still here is a simple mind map when to choose what.

- In most cases while writing code you might use the `Element` class and it's easier to manage and use on paper. Only problem is you must need to define the Element instance for each element before acessing it. Creating new class for each element creates a bit of overhead also.
How ever this is the most standard approach for most cases.

- In case you need to change attribute of many elements at onces say in a table of records it is adviced to use the `Procedural API` as creating a `Element` class for each node is a overkill.

