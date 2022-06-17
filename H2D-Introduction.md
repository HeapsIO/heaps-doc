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

Of course all you get to see here is a black screen or window with no content.

Thus to create content an object instance must be added that inherits from `h2d.Object`.

There are many classes to choose from that all inherit from `h2d.Object` (see the [API](https://heaps.io/api/h2d/Object.html)).

Therefore in the next sample we revisit the code of the ["Hello World" sample](Hello World) and add a bitmap.

Two objects now make up the content here: `h2d.Text` and `h2d.Bitmap` (and as said, both are `h2d.Object`s).


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


### Object
An **Object** (represented by [h2d.Object](https://heaps.io/api/h2d/Object.html)) is the base class of all 2D objects, so any thing that you can *see* on the screen. Therefore `h2d.Object` provides variables like a position (x,y), a scale (scaleX,scaleY), a rotation and methods to change them.
`h2d.Object`s can be any visual objects in the world of the game like for instance the player, enemies, buildings, but furthermore also buttons (`h2d.Interactive`) or a `h2d.Flow` that arranges UI elements and many more.

#### Object trees
Objects can be added to a scene (here `h2d.Scene`) directly or be added to another `h2d.Object` creating an *object tree*.
The child objects will inherit the transformations of the parent object they have been added to. This means whenever the parent transforms (changing the position, rotating, making it transparent or invisible etc.) these transformations *also* apply to all the children that have been added to this parent object.

```haxe
var a = new h2d.Object();              // object not added yet
s2d.addChild(a);                       // now adding `a` to `s2d` (the currently active 2D scene)

var b = new h2d.Object( s2d );         // `b` is added directly via constructor to `s2d`
b.x = 100;

var c = new h2d.Object( b );           // `c` is added to `b` which has been added to a scene (`s2d`)
c.x = 10;                              // relative to parent `b`

trace( c.getAbsPos().getPosition() );  // the absolute x-position of `c` will be 110, because it "travels" along with its parent `b`
```

`h2d.Object` is documented in detail in the [API](https://heaps.io/api/h2d/Object.html).

### Scene
The **Scene** (represented by [h2d.Scene](https://heaps.io/api/h2d/Scene.html)) is a special object which is at the root of the scene tree. In [hxd.App](https://heaps.io/api/hxd/App.html) it is accessible by using the `s2d` variable. You will need to add your objects to the scene before they can be displayed. The Scene also handles events such as clicks, touch, and keyboard keys.
```haxe
class Myapp extends hxd.App
{
    override private function init():Void
    {
        super.init();
        s2d;                            // the current scene
        var myscene = new h2d.Scene();  // create a new scene
        setScene(myscene);              // set it as the current scene
        s2d;                            // is now newscene

        var myobj2 = new h2d.Object();
        s2d.addChild(myobj2);

        var myobj = new h2d.Object(s2d);// add myobj to s2d by passing s2d as parameter

    }
}
```

## Example

Now that the basic concepts have been introduced, let's write a small Heaps application:

```haxe
    class Main extends hxd.App {
        var bmp : h2d.Bitmap;
        override function init() {
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
