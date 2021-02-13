# Hello Hashlink

[HashLink](https://hashlink.haxe.org/) is a virtual machine for Haxe. It can be used to build native desktop applications as well mobile platforms (Android / iOS) and consoles.

Now that you have [installed](https://github.com/HeapsIO/heaps/wiki/Installation) both Heaps and Visual Studio Code, let's create a new Heaps application.

## Differences with the [[Hello World]] tutorial
* `compile.hxml` needs a different compilation target, `hl`, and other libraries to manage rendering
* There's no need for an `index.html` file
* The launch.json file is different to handle hashlink


## Concepts
* Heaps uses a `compile.hxml` file to tell it what to compile, how and where to compile it, and which libraries to include.
* The entry point of a Haxe program is a class containing a static function called `main`.
* A simple heaps directory structure will look like this:
```
.
├── .vscode/
│   └── launch.json
├── res/
├── src/
│   └── Main.hx
└── compile.hxml
```

## Create your project

* Create a new directory named `helloHeaps`
* Create a new file called `compile.hxml`
* Add the following lines to your newly created file
```
-cp src
-lib heaps
-lib hlsdl
-hl hello.hl
-main Main
```

 - ``-cp src`` Tells haxe where to search for your code files
 - ``-lib heaps`` Tells haxe to import the heaps library
 - ``-lib hlsdl`` Tells haxe to import the hlsdl rendering library
 - ``-hl hello.hl`` Tells haxe to compile to hashlink bytecode in the project directory
 - ``-main Main`` Tells haxe that Main.hx is your entry point

The `-lib hlsdl` tells Heaps to compile with SDL/OpenGL support. If you are on Windows you can use `-lib hldx` instead.

## Open with VSCode

At this point, you can open the `helloHeaps` folder with VSCode by launching VSCode and navigating the main menu `File > Open Folder`

## Create Hello World example

Create a new `Main.hx` in a `src` folder in your project directory and put the following content

```haxe
class Main extends hxd.App {
	override function init() {
		var tf = new h2d.Text(hxd.res.DefaultFont.get(), s2d);
		tf.text = "Hello Hashlink !";
	}
	static function main() {
		new Main();
	}
}
```

This example creates a Heaps Text component, adds it to the 2D scene `s2d` and set its text.

## Compile and run Output
To be able to compile and debug your application directly from vscode, you need to create a launch task.

If it does not already exist, create a `.vscode` directory in your project folder and create a new file called `launch.json`.

Add the following code to the file:
```json
{
	"version": "0.2.0",
	"configurations": [
        {
            "name": "HashLink",
            "request": "launch",
            "type": "hl",
            "cwd": "${workspaceFolder}",
            "preLaunchTask": {
                "type": "haxe",
                "args": "active configuration"
            }
        }
	]
}
```

Now, by hitting `F5`, the project will compile and run.

![image](https://user-images.githubusercontent.com/1022912/45916979-06d3ef00-be6f-11e8-9d5c-bc24023a7a66.png)

To compile without running, hit `Ctrl-Shift-B` and select `haxe : active configuration`

If everything works well, you should now have a `hello.hl` file created in your project folder.

You can customize the default window size by adding the following to your `compile.hxml`:

```
-D windowSize=1366x768
```

## Debugging

You can put breakpoints into your Heaps application by clicking in the margin to left to the line number in your source code. You can then start again the problem and see it break at the desired position.

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
