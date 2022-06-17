# Introduction 

The smallest code for a Heaps application is the following:

```haxe
class Main extends hxd.App {
    static function main() {
        new Main();
    }
    public function new() {super();}
}
```

Of course all you get to see here is a black screen or window with no content. Thus to create content an object instance must be added that inherits from `h2d.Object`. There are many classes to choose from that all inherit from `h2d.Object` (see the [API](https://heaps.io/api/h2d/Object.html)). Therefore in the next sample we revisit the code of the ["Hello World" sample](Hello World) and add a bitmap. Two objects now make up the content here: `h2d.Text` and `h2d.Bitmap` (and as said, both are `h2d.Object`s).

```haxe
    class Main extends hxd.App {
        var bmp : h2d.Bitmap;
        override function init() {
            var tf = new h2d.Text(hxd.res.DefaultFont.get(), s2d);
            tf.text = "Hello World !";

            // allocate a Texture with red color and creates a 100x100 Tile from it
            var tile = h2d.Tile.fromColor(0xFF0000, 100, 100);
            // create a Bitmap object, which will display the tile
            // and will be added to our 2D scene (s2d)
            bmp = new h2d.Bitmap(tile, s2d);
            // modify the display position of the Bitmap sprite
            bmp.x = s2d.width * 0.5;
            bmp.y = s2d.height * 0.5;
        }
        // on each frame
        override function update(dt:Float) {
            // increment the display bitmap rotation by 0.1 radians
            bmp.rotation += 0.1;
        }
        static function main() {
            new Main();
        }
    }
```

With overriding `hxd.App`'s `update` method Heaps allows us to define what should happen each frame.
The underlying `h2d.Object` provides us with the basics to change the bitmaps position and rotation.

(`h2d.Tile` will be discussed later in another section.)

