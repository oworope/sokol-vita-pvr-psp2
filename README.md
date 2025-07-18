# PS Vita OpenGL ES 2.0 App using sokol
Example homebrew app for psvita using sokol_gfx and OpenGL ES 2.0.

This code is a fork of [psvita_sokol_example](https://github.com/valiet/psvita_sokol_example) ported to PVR_PSP2.

### Setup:
- `cmake -S . -B builddir`
- `cmake --build builddir`

### Notes:
- Examples how to use the sokol library are found at [sokol_samples](https://github.com/floooh/sokol-samples).

- The gitignore file is ignoring all files using `*` and then allowing specific files/folders like `!src` `!sce_sys/**`. If you don't like this setup feel free to change the gitignore, but if you keep it be aware in case you add new folders, or files in the root of the project. 
