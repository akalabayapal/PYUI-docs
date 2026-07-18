# Dynamic Component Lifecycle

`PYUI 6.0` introduced using components dynamically. Now you can `add` and `remove` components dynamically from `Python Runtime`.

This page covers

1. How to register and unregister custom nodes in PYUI Python Runtime
2. Adding and Removing Components
3. Adding and Remving custom HTML content


## Adding and Removing custom node in PYUI

`PYUI` allows to register or un-register nodes to `Python Runtime` if they are created dynamically. This are internally used in `Component` class but you might need it separately many times, mostly while working with `custom syscalls`.

### Registering a Unmanged Node

    self.pyui.RegisterUnmanagedNode(id,tag)

    # Example

    self.pyui.RegisterUnmanagedNode("dyn_node","div")

### Un-Registering a Unmanaged Node

    self.pyui.UnRegisterUnmanagedNode(id)

    # Example 

    self.UnRegisterUnmanagedNode('dyn_node')

**Note:** Un-register function automatically cleans callbacks associated with the given id

## Adding Component to UI

1. For adding Component to UI. The component must be well defined inside the `Components` folder.If not `PYUI` will fail to create `Component` instance

    Example Component:
        
        <ComponentFile text="Default text" place="Default Placeholder">
            <Text id='' innerText='{text}'></Text>
            <Input placeholder='{place}'></Input>
        </ComponentFile>


2. Make a `Component` instance

        c = Component('component_path_without_extension_relative_to_component_folder')

        # Example
        c = Component(self.pyui,'form')
        c = Component(self.pyui,'dash/admin')

Try to use `POSIX` style path, if not used `PYUI` auto converts it internally to keep the app cross platform.

3. Adding Components to UI

    - **Appending at end:** Use `c.appendComponent()` to add the component to end of the parent id.

            new_id = c.appendComponent('parent_id') # defaults will be used
        
        or,

            # props supplied
            new_id = c.appendComponent('parent_id', text='Hello World',place="New Placeholder") 

        You can use `new_id` as base `id` for the `component`. Just like statically dynamically all the components get `name mangled` with this `new_id`. See [Reusing Layouts using Components](/PYUI%20XML%20LAYOUT%20GUIDE/04_PYUI_Components/) to know how to reference the inner elements

    - **Adding to Top of parent id:** Use `c.addComponentTop()` to add component to top of the parent id.

            new_id = c.addComponentTop('parent_id')

    - **Adding component after a element:** Use `c.addComponentAfter()` to add a component after a certain element.

            new_id = c.addComponentAfter('preceeding_id')


## Deleting a Component

Use `c.removeComponent()`, where `c` is a component instance (see above) to remove components from UI.

#### Example

    c.removeComponent('component_id') 

    # use the id returned during the creation of the thread to remove component

**Note:** This function can remove nodes even if it is not a `Component` so this can be reused in general to remove any `ID` from UI.


## Adding custom HTML to UI

`PYUI 0.6` have loosened the strictness of direct HTML usage for better `DX` and considering how often users might need to add arbitary objects to UI. But, directly mutating HTML might be still harmfull as it is not verified by `Compiler`. So a safety approach is implemented by making `HTMLObject` class.

    generator,id_of_obj = HTMLObject(self.pyui,tag_name,innerContent,**attributes)

    # Example

    generator,id_obj = HTMLObject(self.pyui,'p','Inner text',hidden=True)

This do not add the content to UI. To add this to UI you need to send a `syscall` 

1. Make Send JSON


        toSend = {'parent_id':parentID/preceeding ID,'html_to_add':generator(),"mode":mode}

    - **parent_id:** Stores the parent id/preeceeding element id to which the Element is added.
    - **html_to_add:** This is the html string to be added. Use the generator function returned by `HTMLObject`.
    - **mode:** Where to add the element

        1. **append:** Added to the end of the parent id
        2. **top:** Added to the TOP of the parent id
        3. **after:** Added after a given preeceeding ID

2. Send the Syscall

        self.pyui.sendSyscall('ADD_NODE_END',json.dumps(toSend))

### Few important notes

1. In `HTMLObject` when the `generate()` function is called only then the given node id is registered, it is done so that there are no dangling nodes made until you actually push the html to layout.

2. It is not strictly said that you can't add element inside element but it is better to not write InnerHTML manually and it is better to chain `HTMLObjects`. We will see this in next section. 

3. When `generate()` is called the node is auto-registered with `PYUI Python Runtime`, you are free to use them using `Element` class or using other ways to reference the node.

4. However if the `Node` is deleted later you need to manually Unregister the node using `PYUI's` function (we will see it soon).

5. The adding of custom HTML is intentally made bit elongated to make users use the `Component` as they are much safer. However for small cases like adding `option` to `selector', creating new component is a overkill and there this method is much more suitable.

## Chaining HTMLObjects

Let us understand this using a example. For example we need to add `button` and a `p` inside a `div` and add the whole thing to UI

1. First start from inner Nodes and build up. First make the `button` and the `p`

        gen_btn,id_btn = HTMLObject(self.pyui,'button','Click ME')
        gen_p,id_p = HTMLObject(self.pyui,'p','Click here for magic!!!')

        # this generates the total html also registers the ID
        gen_innerhtml = gen_btn() + gen_p()

2. Make DIV and add the created HTML

        gen_div,id_div = HTMLObject(self.pyui,'div',gen_innerhtml)

3. Send the syscall as done before to add the div to the UI

**Note:** As soon as you generate the html and store it in `gen_innerhtml` the nodes are registered if you do not directly or indirectly push them to UI using syscall then those nodes will be `dangling` with no UI counterpart.

## Removing HTML Contents

You can delete a HTML node using `REM_NODE` syscall and then use `self.pyui.UnRegisterUnmangedNode(id)` to clean the callbacks

1. Send the `REM_NODE` syscall to clean the node from UI.

        toSend = {'id':id_to_delete}
        self.pyui.sendSyscall('REM_NODE',json.dumps(toSend))

2. Unregister the Node

        self.pyui.UnRegisterUnmanagedNode('id')



