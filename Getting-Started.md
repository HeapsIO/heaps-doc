In order to get started with Heaps you'll need to do the following:

* install Haxe 3.2+, available on http://haxe.org
* install Heaps by running `haxelib git heaps https://github.com/ncannasse/heaps.git` for the bleeding edge version 
* setup your IDE in order to build Haxe content, as documented [here](http://haxe.org/documentation/introduction/editors-and-ides.html)

You should then be ready to run your first example:
```haxe
    class Main extends hxd.App {
        var bmp : h2d.Bitmap;
        
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

* If you compile for Javascript,
  * you will also have to create an index.html that includes your .js haxe output.
  * Also put a `<canvas id="webgl"></canvas>` in body of the HTML-file.
  * If not in debug mode (`-debug` compiler flag) add `-dce no` flag to prevent execution failure.
* If you compile for Flash, make sure to specify the Flash version to be at least 11.8 (with `-swf-version 11.8`) which is required for Heaps.

You should now be able to compile and display the example. It should show you a rotating red square.

Several examples for both 2D and 3D are available in the heaps [samples directory](https://github.com/ncannasse/heaps/tree/master/samples).