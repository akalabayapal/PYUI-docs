# Getting Started with PYUI Layout XML

PYUI uses a strict xml layout for defining the layout.Although the layout internally gets compiled to pure html, it was important to only allow xml for clearer, safer and more coherent layout designs.

## Why compilation chosen over loose HTML(HyperText Markup Language)

1. **Safer designs than using html:** Html allows non-closing tags and non-strict envioronment.Eventhough it is comfortable for developers it gets hard handle them as projects grow.Using a coherent xml layout pushes developer to use clean xml from stage 1 of development, that help projects to scale better 

2. **Easier compilation:** It is harder to compile the loose html into a proper tree(needed for layout optimisations) and to catch errors in layout.

3. **Easier Component Reusibility:** Standard HTML do not support component re-use, but PYUI component system does.Safer XML layout made it easier to implement this feature

## Layout System

PYUI Layout System is separated in 3 components

1. **Forms:**Each file in main layout folder represents a form. Equivalent to .NET Framework Forms. Each form is completetely isolated from one another.InterForm communication will need IPC or other interprocess commununication teachniques

2. **Window/Views:**This are the inter-form components.At a single time only one window canbe shown.But they can be changed easily using the PYUI python api (discussed later).

3. **Re-useable Components:**PYUI supports the component-reusage using *Include* tag (discussed later) with props. The components must be stored strictly inside **./Components** folder

## Few Important Rules to write the layout

If you know how to write HTML you know how to write PYUI Layout xml but with few important points:

1. The layout is attribute-centric, that means even text inside tag is written as a attribute than inside tags.
2. For writing the values of attribute only **"** is supported **'** is strictly ignored and compiler will consider it error or layout will break
3. The layout file name and it's code in **./code** folder name must be same v1.0 do not support dynamic routing.Although it is not applicable for components. Example:index.xml layout must have corresponding index.py in the **./code** folder to execute or it will fail on **Runtime**.


