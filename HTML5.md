# HTML5 

Heaps is able to compile to JavaScript and therefore it can run in the web browser.

See the following exaxmple on how to prepare your heaps project for HTML5.

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