# Configuring Tailwind

`PYUI` supports Tailwind directly.You can start using `Tailwind` in your project just by following few steps.

1. Grab a stable tailwind executable from [TailwindCss Releases](https://github.com/tailwindlabs/tailwindcss/releases/)

2. Enable Tailwind inside your `settings.py` file under `CompilerSettings` class

    class CompilerSettings:

            TAILWIND_ENABLED = True # <-- Change to True

3. Now when you execute `PYUI.buildtools` add a extra `--settings settings.py --tailwindpath <your-tailwind-exe-path>`

The buildtools passes the html files to tailwind to compile `global.css`.Now you can use any tailwind freely.