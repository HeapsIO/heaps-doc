# Introduction

The code samples in the first pages in the H2D section are coded without any external resources allowing to **just copy and paste** the code and compiling it directly on your machine.

Let's recap now. The smallest code for a Heaps application is the following:

```haxe
class Main extends hxd.App {
    static function main() {
        new Main();
    }
}
```
Of course all you get to see here is a black screen or window with no content. Thus to create content an instance of a class (an object) must be added that inherits from `h2d.Object` (to make 2D content).

## Adding content
There are many classes to choose from that all inherit from `h2d.Object` (see the [API](https://heaps.io/api/h2d/Object.html)). Therefore in the next sample we revisit the code of the [[Hello World]] sample and add a bitmap. Two objects now make up the content here: `h2d.Text` and `h2d.Bitmap` (and as said, both are `h2d.Object`s). The bitmap requires a tile (`h2d.Tile`) as graphical resource (which will be explained in a later section). Here we can generate the needed tile from code using `fromColor(...)`.

By the way this is a first straight dive into the Heaps engine! All features used here will be explained in detail in the following sections!

### Code

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

With overriding `hxd.App`'s `update` method Heaps allows us to define what should happen each frame. (`dt` stands for the number of milliseconds that have passed since the last time the `update` method has been called.)
The underlying `h2d.Object` (from which `h2d.Bitmap` inherits) provides the bitmap with features to change its position and rotation.

### Screenshot

![h2d_introduction_helloworld2](https://user-images.githubusercontent.com/88530062/174428357-45f857ed-30bf-450d-99b6-72051f5b0b83.png)

The [next section](Objects) will discuss `h2d.Object` a bit closer.

And as said `h2d.Tile` which the bitmap required will be discussed in a later section: [[Graphical surfaces]].
