# HXML flags

Common `-D` flags that can be used with Heaps:

```hxml
# flag to define the size of the window and default scene size:
-D windowSize=800x600

# define the window/process name
-D windowTitle=My game made with Heaps.io

# define the path to the game assets/resources
# if unset, the default path is "res"
-D resourcesPath=res

# other known flags (deprecated/legacy code)
-D heaps_emit_tile_buffering

```

Common flags coming from Haxe

```hxml
# dead code elimination (code that cannot be reached will be ditched):
-D dce=full

# Static analyzer, see: https://haxe.org/manual/cr-static-analyzer.html
-D analyzer-optimize
```
