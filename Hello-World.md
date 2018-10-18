# Hello World

Now that you have [installed](https://github.com/HeapsIO/heaps/wiki/Installation) both Heaps and Visual Studio Code, let's create a new Heaps application.

## Setup compilation options

* Create a new directory `helloHeaps` 
* Create into it a file `compile.hxml`: this file will contain your Haxe compilation parameters and the libraries you are using.
* Edit the `compile.hxml` (with VSCode or any other text editor) to set the following content:
```
-lib heaps
-js hello.js
-main Main
-debug
```

This will tell the compiler that we are using the library Heaps, that we will compile to JavaScript `hello.js` output, and that our main class should be `Main.hx` file.

The `-debug` file allows generation of source maps in order to be able to debug your JS application.

## Open with VSCode

At this point, you can open the `helloHeaps` folder with VSCode by launching VSCode and navigating the main menu `File > Open Folder`

## Create Hello World example

Create a new `Main.hx` in the directory and put the following content

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

## Compile Output

In order to compile, you need to create a Build task with VSCode, this can be done by going into the VSCode menu and select `Terminal > Configure Default Build Task`

Select `haxe : active configuration`

You can now compile with `Shift-Ctrl+B` (or by going `Terminal > Run Build Task`)

If everything works well, you should now have both `hello.js` and `hello.js.map` files created in your `helloHeaps` folder:

![image](https://user-images.githubusercontent.com/1022912/45916520-e6ecfd00-be67-11e8-925c-a762c7950045.png)

## Run in Browser

In order to run our example, we need to embed it into a web page.
Create a file named `index.html` with the following content:

```html
<html>
<head><meta charset="utf-8"/><title>Hello Heaps</title></head>
<body style="margin:0;padding:0;background-color:black">
	<canvas id="webgl" style="width:100%;height:100%"></canvas>
	<script type="text/javascript" src="hello.js"></script>
</body>
</html>
```

In order to Run with Chrome, you need **Chrome Debugger** VSCode extension. Open again the Extension manager and search for Chrome to install it:

![image](https://user-images.githubusercontent.com/1022912/45916600-0fc1c200-be69-11e8-8c4e-19cb5212d85a.png)

Once installed, press `F5` or do `Debug > Start Debugging`.
This should give you the choice to Debug with Chrome. If not, restart VSCode so the extension is activated.

If you click on `Chrome` label, it will open you a `.vscode/launch.json` similar to the `tasks.json` we had earlier for compiling.

```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"type": "chrome",
			"request": "launch",
			"name": "Launch Chrome against localhost",
			"url": "http://localhost:8080",
			"webRoot": "${workspaceFolder}"
		}
	]
}
```

You can either setup a web server on `localhost:8080` to display the content, or either simply replace the `url` using the following value:

```
    "url": "file://${workspaceFolder}/index.html",
```

Now Run again (through the menu or `F5`) and the browser should open and display `Hello World`

![image](https://user-images.githubusercontent.com/1022912/45916668-43511c00-be6a-11e8-8e2c-0d280dedebef.png)

## Debugging

You can put breakpoints into your Heaps application by clicking in the margin to left to the line number in your source code. You can then start again the problem and see it break at the desired position.

![image](https://user-images.githubusercontent.com/1022912/45916676-6bd91600-be6a-11e8-99a1-b15567ee4ec7.png)

## Compile-and-Run

If you want to make sure that compilation is done every time you press `F5`, you can edit your `.vscode/launch.json` file by adding a `preLaunchTask` such as the following example:

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

Now is the time to do more!

Use the menu to explore Heaps features and APIs.



