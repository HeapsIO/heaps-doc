# HashLink

[HashLink](https://hashlink.haxe.org/) is a virtual machine for the Haxe programming language.  By targeting HashLink you are also able to generate Native C code for your project.

Heaps is able to support both SDL and DirectX. In order to specify which you'd like to use, you simple need to include the appropriate library when compiling your project.

For DirectX (optionally DirectX 12)
```hxml
-lib hldx
#-D dx12
```

For SDL
```hxml
-lib hlsdl
```

## For HashLink/JIT:

To compile for HashLink use the following example.

```hxml
# class paths
-cp src

# entry point
-main Main

# libraries
-lib heaps
-lib format
-lib hldx
#-D dx12
#-lib hlsdl

# output
-hl bin/game.hl
```

And run with:

```
hl bin/game.hl
```

## Compile for HashLink/C:

Alternatively, you can compile in hl/c, and compile with a C compiler.

```hxml
# class paths (same as hl/jit)
-cp src

# entry point (same as hl/jit)
-main Main

# libraries (same as hl/jit)
-lib heaps
-lib format
-lib hldx
#-D dx12
#-lib hlsdl

# output
-hl bin/hlcout/game.c
```

### GCC

If you're using GCC, you can build with:

```
gcc -O3 -o myapp -std=c11 -I out out/main.c -lhl -lheaps.hdll -lui.hdll -lfmt.hdll [-L/path/to/required/hdll]
```

For template, see also [Hashlink#706](https://github.com/HaxeFoundation/hashlink/pull/706).

### Visual Studio

If you're using Visual Studio, add the following line in your hxml file, and compile using `bin/game.sln`:

```hxml
# use a Visual Studio template
-D hlgen.makefile=vs2019
-D hlgen.makefilepath=bin
#-D hlgen.makefile.jumbo=false
```
