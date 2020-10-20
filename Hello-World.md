# Hello World

Now that you have [installed](https://github.com/HeapsIO/heaps/wiki/Installation) both Heaps and Visual Studio Code, let's create a new Heaps application.

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
-js hello.js
-main Main
-debug
```

* `-cp src` Tells haxe where to search for your code files
* `-lib heaps` Tells haxe to import the heaps library
* `-js hello.js` Tells haxe to compile to javascript in the project directory
* `-main Main` Tells haxe that Main.hx is your entry point
* `-debug Tells` haxe to run in debug mode

This will tell the compiler that we are using the library Heaps, that we will compile to JavaScript `hello.js` output, and that our main class should be `Main.hx` file.

The `-debug` file allows generation of source maps in order to be able to debug your JS application.

## Open with VSCode

At this point, you can open the `helloHeaps` folder with VSCode by launching VSCode and navigating the main menu `File > Open Folder`

## Create Hello World example

Create a new `Main.hx` in a `src` folder in your project directory and put the following content

```haxe
class Main extends hxd.App {
	override function init() {
		var tf = new h2d.Text(hxd.res.DefaultFont.get(), s2d);
		tf.text = "Hello World !";
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
			"type": "chrome",
			"request": "launch",
			"name": "Launch Chrome against localhost",
			"url": "file://${workspaceFolder}/index.html",
			"webRoot": "${workspaceFolder}",
			"preLaunchTask": {
				"type" : "haxe",
				"args" : "active configuration"
			}
		}
	]
}
```

Now, in your project directory, create a file `index.html` and add the following code to it:

```html
<!DOCTYPE>
<html>
<head>
	<meta charset="utf-8"/>
	<title>Hello Heaps</title>
	<style>
		body { margin:0;padding:0;background-color:black; }
		canvas#webgl { width:100%;height:100%; } 
	</style>
</head>
<body>
	<canvas id="webgl"></canvas>
	<script type="text/javascript" src="hello.js"></script>
</body>
</html>
```


Now, by hitting `F5`, the project will compile and run.

![image](https://user-images.githubusercontent.com/1022912/45916668-43511c00-be6a-11e8-8e2c-0d280dedebef.png)

To compile without running, hit `Ctrl-Shift-B` and select `haxe : active configuration`

If everything works well, you should now have both `hello.js` and `hello.js.map` files created in your project folder:

![image](https://user-images.githubusercontent.com/1022912/45916520-e6ecfd00-be67-11e8-925c-a762c7950045.png)

## Debugging

In order to Run with Chrome, you need **Chrome Debugger** VSCode extension. Open again the Extension manager and search for Chrome to install it:

![image](https://user-images.githubusercontent.com/1022912/45916600-0fc1c200-be69-11e8-8c4e-19cb5212d85a.png)

You can put breakpoints into your Heaps application by clicking in the margin to left to the line number in your source code. You can then start again the problem and see it break at the desired position.

![image](https://user-images.githubusercontent.com/1022912/45916676-6bd91600-be6a-11e8-99a1-b15567ee4ec7.png)
