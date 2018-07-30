# Animation

Creating an animated sprite in H2D is relative easy.

Instead of using [`h2d.Bitmap`](https://github.com/ncannasse/heaps/blob/master/h2d/Bitmap.hx) to display a single Tile, you can use [`h2d.Anim`](https://github.com/ncannasse/heaps/blob/master/h2d/Anim.hx) to display a list of tiles that will automatically be played:

```haxe
// creates three tiles with different color
var t1 = h2d.Tile.fromColor(0xFF0000, 30, 30);
var t2 = h2d.Tile.fromColor(0x00FF00, 30, 40);
var t3 = h2d.Tile.fromColor(0x0000FF, 30, 50);

// creates an animation for these tiles
var anim = new h2d.Anim([t1,t2,t3],s2d);
```

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

