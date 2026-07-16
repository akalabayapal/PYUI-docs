# Extending PYUI Compiler using settings.py

PYUI uses strict compilation time validation to protect your layout and give safer and stable layout.So only few tags are allowed right now in the compiler, but you can safely use more HTML tags directly by adding few lines in `settings.py` file in your project directory.

## Elemental and Container Tags

PYUI differentiates between two types of tags.

1. **Elemental Tags:** This are the buttons,inputs,Text etc. In this tags all properties even text is attribite defined. If you add elements inside this tags it will be ignored by compiler. This Tags are defined in `TAG_RULES_HASHMAP` in `CompilerSettings` class in `settings.py`.

2. **Container Tags:** This are the tags to hold everal Elemental tags inside them.They can contain other Elemental or Contianer Tags inside them. This Tags are additionally defined in `LAYOUT_CONTAINER_TAGS` in `CompilerSettings` class in `settings.py`.


## Understanding Layout validation

Before diving into how to add tags and attributes in `PYUI XML` it is important to understand role of three objects in `settings.py`.

1. PYUI compiler makes a document tree and traverses to check if all nodes Tags are present in either `TAG_RULES_HASHMAP` or `LAYOUT_CONTAINER_TAGS`. If it doesn't it stops compilation and throws error.

2. PYUI Compiler also checks the attributes of the tags and checks if those attributes are allowed in the `TAG_RULES_HASHMAP` dictionary under the particiular tag.

3. PYUI Converter uses `HTML_TAG_CONVERSION_MAP` to change PYUI XML to HTML using this map.All attributes except `style-class` and innerText remains same.

    - style-class gets changed to class in pure html.
    - innerText gets branched out to make it the actual innerText of that tag.

    

## Adding/Removing a attribute in allowed list

1.Find out the tag in  `TAG_RULES_HASHMAP` dictionary in settings.py under `CompilerSettings` class.

2.You will find a repective `set` object for the tag in `TAG_RULES_HASHMAP`. This set contains all allowed attributes for the tag.

3.Add or remove attributes inside the set and save `settings.py`

### Example

Consider the tag `Button`:

        {
        .....
        "Button": {"innerText", "style-class", "id"},
        .....
        }

In button this 3 attributes are allowed.But if I want to add a new attribute to allowed list let's say `attribute_custom`. I need to update it like the following:

        {
        .....
        "Button": {"innerText", "style-class", "id","attribute_custom"}, # <-- Custom attribute added
        .....
        }

That is it.. Now compiler knows to allow `attribute_custom`.

**Note:** To run compilation/hot-reloading with new settings use `--settings settings.py` tag while compilation or hot-reloading.

## Adding New Elemental Tag in PYUI

1. Visit `TAG_RULES_HASHMAP` dictionary. You will find all tags that are allowed.

2. Add your new tag inside dictionary.

3. Add the html equivalent tag of it in `HTML_TAG_CONVERSION_MAP`.

### Example

Consider `TAG_RULES_HASHMAP`

    TAG_RULES_HASHMAP = {

    ......
    ......
    
    # --- NEW MEDIA TAGS ---
    "img": {"id", "style-class", "src", "alt", "width", "height"},
    "video": {"id", "style-class", "src", "controls", "autoplay", "loop", "muted", "poster", "width", "height"},
    "audio": {"id", "style-class", "src", "controls", "autoplay", "loop", "muted"}

    # ---- NEW TAG ADDED ---- #
    "custom_tag":set() # <--- Custom Tag

    }


Then go to `HTML_TAG_CONVERSION_MAP` and add HTML equivalent tag for `custom_tag`

    HTML_TAG_CONVERSION_MAP = {

        .....
        "br": "br",
        "Para":"p",
        # --- NEW MEDIA MAPPINGS ---
        "img": "img",
        "video": "video",
        "audio": "audio",

        "custom_tag":"h1" # <-- New tag 'custom_tag' will get converted to a static html `h1` upon compilation

    }

That is it. Custom Tag is added. Use [Adding/Removing a attribute in allowed list ](#addingremoving-a-attribute-in-allowed-list) reference to add attributes to newly created tag.

**Note:** To run compilation/hot-reloading with new settings use `--settings settings.py` tag while compilation or hot-reloading.


## Adding New Container Tag in PYUI

For adding a new Container Tag in PYUI first follow steps given in [Adding New Elemental Tag in PYUI](#adding-new-elemental-tag-in-pyui)

Then, visit `LAYOUT_CONTAINER_TAGS` in `settings.py` under `CompilerSettings` class

    LAYOUT_CONTAINER_TAGS = {"main-content", "window","container",
    "pyui","custom_tags" } # <-- `custom_tags` added

**Note:** To run compilation/hot-reloading with new settings use `--settings settings.py` tag while compilation or hot-reloading.


 


