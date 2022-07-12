# Creating simple sprites

To create a sprite we load a `h2d.Tile` into the RAM. From there we can fetch the tile to instantiate a `h2d.Bitmap` which can finally be *placed* inside our scene.

In order to use resources in our `hxd.App` the method `hxd.Res.initLocal();` (or a method alike) must be called once on start-up.

After that a sprite can be created with:
```haxe

new h2d.Bitmap( hxd.Res.mysprite.toTile(), s2d );

```

## Demonstration

There are many ways to create sprites:

### Image resources

![mysprite](https://user-images.githubusercontent.com/88530062/178524612-dc720d4b-409b-4bfa-89cf-40673eb1ff9a.png)
`mysprite.png`

![mysprite_strip](https://user-images.githubusercontent.com/88530062/178524639-f0be34e1-d9bf-43a1-b4bc-7ac79ea92c12.png)
`mysprite_strip.png`

![mysprite_sheet](https://user-images.githubusercontent.com/88530062/178524666-87790e44-05d7-4e74-9ae0-0095a44c289d.png)
`mysprite_sheet.png`

### Code

```haxe
import hxd.Res;

import h2d.Tile;
import h2d.Bitmap;

class SimpleSprites extends hxd.App {
    static function main() {
        new SimpleSprites();
        Res.initLocal();
    }

    override function init() {

        // creating a simple sprite
        var tile = Res.mysprite.toTile();
        var bmp  = new Bitmap( tile, s2d );
        infoLabel(bmp,"a simple sprite");

        // creating a sprite strip (a row of sprites, horizontal)
        var tiles = Res.mysprite_strip.toTile().split( 3 );
        var bmp   = new Bitmap( tiles[1], s2d );
        infoLabel(bmp,"from a strip\n(index = 1)");

        // creating a sprite sheet (a grid of sprites)
        var tiles = Res.mysprite_sheet.toTile().grid( 32 );
        var bmp   = new Bitmap( tiles[1][0], s2d );
        infoLabel(bmp,"from a sheet\n(index: x=1, y=0)");

        // using autoCut (will also punch the background in case it's not transparent)
        var tile = Tile.autoCut( Res.mysprite_sheet.toBitmap(), 32, 32 ); // (!!!) mind size (!!!)
        var bmp  = new Bitmap( tile.tiles[0][0], s2d );
        infoLabel(bmp,"autoCut()\nalso a sheet/grid\n(index: x=0, y=0)");

        // a simple sprite again, but the tile is centered and horizontally flipped
        var tile = tile.tiles[1][0]; // uses the sheet from above
        tile = tile.center();
        tile.flipX();
        var bmp  = new Bitmap( tile, s2d );
        infoLabel(bmp,"centered + flipped");

        // (representation)
        // position all added `h2d.Object`s
        var _y = 0;
        for( o in s2d.getLayer(0) )
            o.setPosition( 250, (++_y)*100 );
    }

    function infoLabel( obj:h2d.Object, infotext:String ){
        var t = new h2d.Text( hxd.res.DefaultFont.get(), obj );
        t.text = infotext;
        t.setPosition( -225, 0 );
    }
}
```

### Directory setup

```
.
├── res/
│   └── mysprite.png
│   └── mysprite_strip.png
│   └── mysprite_sheet.png
├── src/
│   └── SimpleSprites.hx
└── compile.hxml

```

### hxml-file "compile.hxml"

This could be the hxml-file for HashLink:

```
-lib heaps
-lib hlsdl
-cp src
-p res
-main SimpleSprites
-hl simple_sprites_demo.hl

```

### Notes

The method `h2d.Tile.autoCut` punches the sprite from the background color, which will then be transparent. Alternatively simply use image resources that have a transparent background:

![mysprite_punch](https://user-images.githubusercontent.com/88530062/178530736-ebbf3674-638e-4c73-980c-12f829dd2ba0.png)

