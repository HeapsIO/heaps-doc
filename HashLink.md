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

## Compile for HashLink/JIT:

To compile for hl/jit, use the following example.

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

Alternatively, you can compile for hl/c, and then compile C files with a C compiler.

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
cd bin
gcc -O3 -o mygame -std=c11 -I hlcout hlcout/game.c -lhl -lheaps.hdll -lui.hdll -lfmt.hdll [-L/path/to/required/hdll]
```

For template, see also [Hashlink#706](https://github.com/HaxeFoundation/hashlink/pull/706).

### Visual Studio

If you're using Visual Studio, add the following line in your hxml file:

```hxml
# use a Visual Studio template
-D hlgen.makefile=vs2019
-D hlgen.makefilepath=bin
#-D hlgen.makefile.jumbo=false
```

You'll need to set an environment variable `HASHLINK` point to your hashlink installation folder.

You can then compile using `bin/game.sln`, or with this sample makefile:

```makefile
PLATFORM=x64
MS_BUILD?="C:/Program Files (x86)/Microsoft Visual Studio/2019/Community/MSBuild/Current/Bin/MSBuild.exe"
#MS_BUILD?="C:/Program Files/Microsoft Visual Studio/2022/Community/MSBuild/Current/Bin/MSBuild.exe"
CONFIG?=Release

all:
	$(MS_BUILD) game.sln -t:game -nologo -verbosity:minimal -property:Configuration=$(CONFIG) -property:Platform=$(PLATFORM)
```

## Distribution

See [Hashlink Wiki Distribution & Packaging](https://github.com/HaxeFoundation/hashlink/wiki/Distribution-&--Packaging).
