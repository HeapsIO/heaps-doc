Common `-D` flags that can be used with Heaps:

```
# flag to define the size of the window and default scene size:
-D windowSize=800x600

# define the window/process name
-D windowTitle=My game made with Heaps.io

# define the path to the game assets/resources
# (actually not recommended, just use `-p res` insead,
# but which demands assets to be in a directory called `res`)
-D resourcesPath=assets
# so rather use:
-p res

# other known flags (deprecated/legacy code)
-D heaps_emit_tile_buffering

```

Common flags coming from Haxe
```
# dead code elimination (code that cannot be reached will be ditched):
-D dce=full

# Tail Recursion Elimination (TRE): Replace recursive programming
# (only `static` and `final` methods, as well as local/arrow functions)
# (https://haxe.org/manual/cr-tail-recursion-elimination.html):
#-D analyzer-optimize
```

---
## More

```
# Popular resolutions
-D windowSize=1920x1080
-D windowSize=1280x720
-D windowSize=1024x768
```