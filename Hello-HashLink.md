[HashLink](https://hashlink.haxe.org/) is a virtual machine for Haxe. It can be used to build native desktop applications as well mobile platforms (Android / iOS) and consoles.

This tutorial requires you to have completed already the [[Installation]], including the HashLink specific parts.

Please also read [[Hello World]] as we follow the same steps but with some changes.

## Setup compilation options

Create a new folder `helloHL` and a `compile.hxml` file similar to what is done in [[Hello World]] example.

But instead of `-js hello.js`, instead use `-hl hello.hl`, your HXML file should then look like this:

```
-lib heaps
-lib hlsdl
-hl hello.hl
-main Main
```

_(Note: unlike JavaScript, you can debug in HL without compiling with `-debug`)_

The `-lib hlsdl` tells Heaps to compile with SDL/OpenGL support. If you are on Windows you can use `-lib hldx` instead.

## Compile Output

Follow instructions on [[Hello World]] regarding compilation, and use the same `Main.hx` source file.
If everything works fine you should get a `hello.hl` file compiled and ready to run.

![image](https://user-images.githubusercontent.com/1022912/45916898-81037400-be6d-11e8-8d57-0e13778c4064.png)

## Run and Debug

Select in menu `Debug > Add Configuration`. This should give you the choice to Debug with `HashLink`. If not, make sure to install `HashLink Debugger` extension.

If you click on `HashLink` label, it will open you a `.vscode/launch.json`. Change its content to the following:

```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "HashLink",
			"request": "launch",
			"type": "hl",
			"hxml": "compile.hxml",
			"cwd": "${workspaceRoot}",
			"preLaunchTask": {
				"type" : "haxe",
				"args" : "active configuration"
			}
		}
	]
}
```

Note in particular that you need to set the correct `hxml` file that contains your compilation parameters.

Now `Run` using `F5` and it should open a native window showing `Hello World`:

![image](https://user-images.githubusercontent.com/1022912/45916979-06d3ef00-be6f-11e8-9d5c-bc24023a7a66.png)

You can customize the default window size by adding the following to your `compile.hxml`:

```
-D windowSize=1366x768
```

Interactive debugging should also be working if you setup a breakpoint as in Hello World sample:

![image](https://user-images.githubusercontent.com/1022912/45917022-5b776a00-be6f-11e8-9319-77d4e36ea3c2.png)

## Compile and Run natively

The `hello.hl` file contains bytecode that can be run with the HashLink virtual machine using `hl hello.hl`. It does give quite good performances and have been proven by successful commercial games such as Northgard or Dead Cells.

However, it is also possible to compile the HashLink code using a native compiler. This allows to compile for consoles and mobile.

This is done by changing `compile.hxml` to use `-hl out/main.c` instead of `-hl hello.hl`. 

This will create a directory `out` containing a lot of generated C code that needs to be built using a native compiler and linked to the same HashLink runtime that the HashLink virtual machine is using.

Compiling on mobile and console thus requires some knowledge with each platform compilers and build systems:
 * for iOS, look at [this thread](https://github.com/HaxeFoundation/hashlink/issues/144)
 * for Android, look at [this thread](https://github.com/HaxeFoundation/hashlink/issues/109)
 * for Consoles (Nintendo Switch, Sony PS4, Microsoft XBoxOne), please contact us at nicolas `@` haxe.org if you are a registered developer for one or several of these


