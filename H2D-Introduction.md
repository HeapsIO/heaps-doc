# Introduction 

Before discussing H2D in-depth, let's introduce a few concepts that we will use throughout the documentation:

## In-Memory Bitmap
_A Bitmap (represented by [hxd.BitmapData](https://github.com/ncannasse/heaps/blob/master/hxd/BitmapData.hx)) is a picture stored in local memory which you can modify and access its individual pixels. In Heaps, before being displayed, a Bitmap needs to be turned into a Texture_  


## Texture
_A Texture (represented by [h3d.mat.Texture](https://github.com/ncannasse/heaps/blob/master/h3d/mat/Texture.hx)) whose per-pixel data is located in GPU memory. You can no longer access its pixels or modify it in an efficient way. But it can be used to display 3D models or 2D pictures._


## Tile
_A Tile (represented by [h2d.Tile](https://github.com/ncannasse/heaps/blob/master/h2d/Tile.hx)) is a sub part of a Texture. For instance a 256x256 Texture might contain several graphics, such as different the frames of an animated sprite. A Tile will be a part of this texture, it has a (x,y) position and a (width,height) size in pixels. It can also have a pivot position (dx,dy)._


## Tile Pivot
_By default a Tile pivot is to the upper left corner of the part of the texture it represents. The pivot can be moved by modifying the (dx,dy) values of the Tile. For instance by setting the pivot to (-tile.width,-tile.height), it will now be at the bottom right of the Tile. Changing the pivot affects the way Bitmap Sprites are displayed and the way local transformations (such as rotations) are performed._


## Sprite
_A Sprite (represented by [h2d.Sprite](https://github.com/ncannasse/heaps/blob/master/h2d/Sprite.hx)) is the base class of all of H2D displayable objects. A Sprite has a position (x,y), a scale (scaleX,scaleY), a rotation. It can contain other Sprites which will inherit its transformations, creating a scene tree_


## Scene
_The Scene (represented by [h2d.Scene](https://github.com/ncannasse/heaps/blob/master/h2d/Scene.hx)) is a special Sprite which is at the root of the scene tree. In hxd.App is it accessible with the s2d variable. You will need to add your Sprites to the scene before they can be displayed. The Scene also handles events such as clicks, touch, and keyboard keys._


## Bitmap Sprite
_A Bitmap sprite (represented by [h2d.Bitmap](https://github.com/ncannasse/heaps/blob/master/h2d/Bitmap.hx)) is a Sprite that allows you to display an unique Tile at the sprite position, such as in the previous example._


Now that the basic concepts have been introduced, let's get back to our previous example, this time with comments:

```haxe
    class Main extends hxd.App {
        var bmp : h2d.Bitmap;
        override function init() {
            // allocate a Texture with red color and creates a 100x100 Tile from it
            var tile = h2d.Tile.fromColor(0xFF0000, 100, 100);
            // create a Bitmap sprite, which will display the tile
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

We can easily make the Bitmap rotate around its center by changing the tile pivot, by adding the following lines:
	
```haxe
    bmp.tile.dx = -50;
    bmp.tile.dy = -50;
```