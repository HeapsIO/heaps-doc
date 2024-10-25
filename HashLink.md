# HashLink

[HashLink](https://hashlink.haxe.org/) is a virtual machine for the Haxe programming language.  By targeting HashLink you are also able to generate Native C code for your project.

HashLink is able to support both [SDL](https://lib.haxe.org/p/hlsdl) and [DirectX](https://lib.haxe.org/p/hldx). At least one and only one is required.
[OpenAL](https://lib.haxe.org/p/hlopenal) provides using sound.

For SDL
```hxml
-lib hlsdl
```

For DirectX (Windows)
```hxml
-lib hldx
```

OpenAL (for sound)
```hxml
-lib hlopenal
```

More available libraries: https://github.com/HaxeFoundation/hashlink/tree/master/libs


## Compile for HashLink:

To compile for HashLink use the following example.


```hxml
# class paths
-cp src

# entry point
-main Main

# libraries
-lib heaps
-lib hlsdl
#-lib hldx
-lib hlopenal

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
