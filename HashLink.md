# HashLink

[HashLink](https://hashlink.haxe.org/) is a virtual machine for the Haxe programming language.  By targeting HashLink you are also able to generate Native C code for your project.

Heaps is able to support both SDL and DirectX. In order to specify which you'd like to use, you simple need to include the appropriate library when compiling your project.

For DirectX
```hxml
-lib hldx
```

For SDL
```hxml
-lib hlsdl
```

## Compile for HashLink: 

To copmile for HashLink use the following example.


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
 
---