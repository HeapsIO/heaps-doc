# HashLink

[HashLink](https://hashlink.haxe.org/) is a virtual machine for the Haxe programming language.  By targeting HashLink you are also able to generate Native C code for your project.

HashLink is able to support both SDL and DirectX. In order to specify which one you'd like to use, you simple need to include the appropriate library when compiling your project.

For DirectX
```hxml
-lib hldx
```

For SDL
```hxml
-lib hlsdl
```

## Compile for HashLink:

To compile for HashLink use the following example.


```hxml
# class paths
-cp src

# entry point
-main Main

# libraries
-lib heaps
-lib hldx
#-lib hlsdl

# output
-hl bin/game.hl
```


## Hot Reload

Hot reload allows to program new features into a game ***while*** it is running.

Use the following flag to allow that Hot Reload is being used.
```
-D hot-reload
```

Now run your HashLink program with
`hl --hot-reload mygame.hl`

Now everytime you re-compile, the running game also gets updated.

Note that this has its limits. Currently no new fields can be implemented. However it works well for modifying the behaviour of an already existing method (class function), for instance the `update` method and all other functions called inside it.

(See details here: https://github.com/HaxeFoundation/hashlink/wiki/Hot-Reload)
