# Introduction 

## Concepts

### Object
An **Object** (represented by [h2d.Object](https://heaps.io/api/h2d/Object.html)) is the base class of all 2D objects, so any thing that you can *see* on the screen. Therefore `h2d.Object` provides variables like a position (x,y), a scale (scaleX,scaleY), a rotation and methods to change them.
`h2d.Object`s can be any visual objects in the world of the game like for instance the player, enemies, buildings, but furthermore also buttons (`h2d.Interactive`) or a `h2d.Flow` that arranges UI elements and many more.

#### Object trees
Also a `h2d.Object` can contain other `h2d.Object`s which will inherit its transformations, creating an *object tree*.
This means the parent object can add children by `addChild` and whenever it transforms (changing the position, rotating, making it transparent or invisible etc.) these transformations *also* apply to all the children that have been added to this parent object.

```haxe
var myobj = new h2d.Object();
s2d.addChild(myobj);
myobj.x = 100;
myobj.rotation = Math.PI/3;

var mysecondobj = new h2d.Object();
myobj.addChild(mysecondobj);
```

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

### Image
An **Image** (represented by [hxd.res](https://heaps.io/api/hxd/res/Image.html)) is an image resource loaded from the filesystem. It has methods to convert itself into a tile or texture (see below).

```haxe
var myimage = hxd.Res.img.myImage;
// will load myImage.png/jpg/jpeg/gif from <your project folder>/res/img/
var mytile = myimage.toTile();
```

### Tile
A **Tile** (represented by [h2d.Tile](https://heaps.io/api/h2d/Tile.html)) is a sub part of a Texture. For instance a 256x256 Texture might contain several graphics, such as the different frames of an animated sprite. A Tile will be a part of this texture, it has a (x,y) position and a (width,height) size in pixels. It can also have a pivot position (dx,dy).

```haxe
var mycolortile = h2d.Tile.fromColor(0xFF00FF, 100, 100, 1);
var myimagetile = myimage.toTile();
```

#### Tile Pivot
By default a tile **pivot** is to the upper left corner of the part of the texture it represents. The pivot can be moved by modifying the (dx,dy) values of the Tile. For instance by setting the pivot to (-tile.width,-tile.height), it will now be at the bottom right of the Tile. Changing the pivot affects the way bitmaps are displayed and the way local transformations (such as rotations) are performed.



### Bitmap
A **Bitmap** (represented by [h2d.Bitmap](https://heaps.io/api/h2d/Bitmap.html)) is a 2D object that allows you to display a unique Tile at the sprite position.
```haxe
var mybitmap = new h2d.Bitmap(myimagetile);
s2d.addChild(mybitmap);
```

We can easily make the Bitmap rotate around its center by changing the tile pivot, by adding the following lines:

```haxe
    bmp.tile.dx = -50;
    bmp.tile.dy = -50;
```

### Pixels
**Pixels** (represented by [hxd.Pixels](https://heaps.io/api/hxd/Pixels.html)) are a picture stored in local memory which you can modify and access its individual pixels. In Heaps, before being displayed, pixels needs to be turned into a Texture.
```haxe
var mypixels = myimage.getPixels();
var mytile = h2d.Tile.fromPixels(mypixels);
var mypixels = hxd.Pixels.alloc(100, 100, hxd.PixelFormat.ARGB);
```

###  Texture
A **Texture** (represented by [h3d.mat.Texture](https://heaps.io/api/h3d/mat/Texture.html)) whose per-pixel data is located in GPU memory. You can no longer access its pixels or modify it in an efficient way. But it can be used to display 3D models or 2D pictures.
```haxe
var mytex = myimage.toTexture();
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
