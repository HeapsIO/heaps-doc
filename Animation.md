# Animation

Creating an animated sprite in H2D is relative easy.

Instead of using [`h2d.Bitmap`](https://heaps.io/api/h2d/Bitmap.html) to display a single Tile, you can use [`h2d.Anim`](https://heaps.io/api/h2d/Anim.html) to display a list of tiles that will automatically be played:

```haxe
// creates three tiles with different color
var t1 = h2d.Tile.fromColor(0xFF0000, 30, 30);
var t2 = h2d.Tile.fromColor(0x00FF00, 30, 40);
var t3 = h2d.Tile.fromColor(0x0000FF, 30, 50);

// creates an animation for these tiles
var anim = new h2d.Anim([t1,t2,t3],s2d);
```

## Working with spritesheets

One way to create an animation is by using an external spritesheet. One way to do this is by first using the [`hxd.Res`](https://heaps.io/api/hxd/res/index.html) class to load sprite sheet, convert it to a tile using the [`toTile()`](https://heaps.io/api/hxd/res/Any.html#toTile) method, and then convert that into tile array with [`gridFlatten()`](https://heaps.io/api/h2d/Tile.html#gridFlatten). You can then pass the tile array to a new instance of the [`h2d.Anim`](https://heaps.io/api/h2d/Anim.html) class.

```haxe
// creates a tile array by dividing the sprite sheet into a grid
var tileArray = hxd.Res.knight.noBKG_KnightIdle_strip.toTile().gridFlatten(64);

// creates an animation from the tile array
var player = new h2d.Anim(tileArray, s2d);
```

You can learn more about Heaps resource management and using [`hxd.Res`](https://heaps.io/api/hxd/res/index.html) in the [Resource Management]([`hxd.Res`](https://heaps.io/api/hxd/res/index.html)) section of the documentation.

## Properties and methods

The following properties and methods can be accessed on [h2d.Anim](https://github.com/ncannasse/heaps/blob/master/h2d/Anim.hx):

* `speed` : changes the playback speed of the animation, in frames per seconds.
* `loop` : tells if the animation will loop after it reaches the last frame.
* `onAnimEnd` : this dynamic method can be set to be informed when we have reached the end of the animation :

```haxe
anim.onAnimEnd = function() {
	trace("animation ended!");
}
```	

`Anim` instances have other properties which can be discovered by reviewing the [`h2d.Anim`](https://github.com/ncannasse/heaps/blob/master/h2d/Anim.hx) class.

## Using image resources

What you'll probably need for your game is actually using your image resources and sprites.
- This is a small external github sample: https://github.com/Beeblerox/Simplest-Heaps-Examples/tree/master/04_heaps_anim
It uses an image to create a sprite strip/sheet from it.