# Example application

After installation you should be ready to run your first example:

```haxe
class Main extends hxd.App {
	var bmp:h2d.Bitmap;
	
	override function init() {
		var tile = h2d.Tile.fromColor(0xFF0000, 100, 100);
		bmp = new h2d.Bitmap(tile, s2d);
		bmp.x = s2d.width * 0.5;
		bmp.y = s2d.height * 0.5;
	}
	
	override function update(dt:Float) {
		bmp.rotation += 0.1;
	}
	
	static function main() {
		new Main();
	}
}
```

To ensure the program compiles include the Heaps library by adding `-lib heaps` to your compilation parameters.

---

## Compile for Javascript: 

 * You will also have to create an index.html that includes your .js haxe output.
 * Also put a `<canvas id="webgl"></canvas>` in body of the HTML-file.

### Basic hxml

```hxml
# class paths
-cp src

# entry point
-main Main

# libraries
-lib heaps

# output
-js bin/game.js
```
### Basic html

```hxml
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<title>Heaps Game</title>
	<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no, minimal-ui" />
	<meta name="format-detection" content="telephone=no" />
	<meta name="apple-mobile-web-app-capable" content="yes" />
	<meta name="mobile-web-app-capable" content="yes" />
	<style>
		body,html { margin:0; padding:0; background-color:black; width:100%; height:100%; }
		canvas#webgl { width:100%; height:100%; }
	</style>
</head>
<body>
	<canvas id="webgl"></canvas>
	<script src="game.js"></script>
</body>
</html>
```

---

## Compile for HashLink: 

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

## Compile for Flash
Make sure to specify the Flash version to be at least 11.8 (with `-swf-version 11.8`) which is required for Heaps.

### Basic hxml

```hxml
# class paths
-cp src

# entry point
-main Main

# libraries
-lib heaps

# output
-swf bin/game.swf
-swf-version 11.8
```

---

You should now be able to compile and display the example. It should show you a rotating red square.

Several examples for both 2D and 3D are available in the [Heaps samples](samples/).
