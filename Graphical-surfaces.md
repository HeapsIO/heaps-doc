
### Image
An **Image** (represented by [hxd.res](https://heaps.io/api/hxd/res/Image.html)) is an image resource loaded from the filesystem. It has methods to convert itself into a tile or texture (see below). Please make sure to [initialize](https://heaps.io/documentation/resource-management.html) your resources first.

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
