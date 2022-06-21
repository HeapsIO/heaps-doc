# Object
Like shown previously in the introduction to give our Heaps application visual (2D) content we need classes that are build upon `h2d.Object`.

An **Object** (represented by [h2d.Object](https://heaps.io/api/h2d/Object.html)) is the base class of all 2D objects, so any thing that you can *add* to the screen (so to a `h2d.Scene` to be more precise, which mainly is `s2d` when inside `hxd.App`).

`h2d.Object` provides variables like a position (x,y), a scale (scaleX,scaleY), a rotation and methods to change them. However the class itself is *only a **container*** and has nothing to render by itself. For instance `h2d.Flow`, `h2d.Layers` and `h2d.Mask` are all "invisible", too, by that sense. Their job is to *structure in which order* their children objects are rendered.

A sub-class that *does* rendering however is `h2d.Drawable`([[Drawable]]). These objects are the base for any actually visible objects in the world of the game like for instance *the player, enemies, buildings, the landscape*, but furthermore also buttons (`h2d.Interactive`) or a `h2d.Text`.

These are all `h2d` package classes (and sub-classes) that extend `h2d.Object` and can be used to create 2D content:
- Console
- Drawable
  - Anim, Bitmap, Particles, SpriteBatch
  - Graphics
    - Benchmark
  - Interactive
    - Slider
  - Text
    - HtmlText, TextInput
  - TileGroup
    - ScaleGrid
  - Video
- Flow
  - CheckBox, Dropdown
- Mask
  - KeyFrames
- Layers
  - CdbLevel, Scene, ZGroup
- ObjectFollower
- Loader

## Sample

A collection of different objects that all inherit from `h2d.Object` (and `h2d.Drawable`):

![allKindsOfObjects_demo](https://user-images.githubusercontent.com/88530062/174468670-4ea6ddd8-39b7-4491-8a70-fbb49f240594.png)


```haxe
class AllKindsOfObjects extends hxd.App {
    static function main() {new AllKindsOfObjects();}
    function new() {super();}
    override function init() {

        // a mere h2d.Object (no visual qualities => nothing to see here)
        var obj = new h2d.Object( s2d );
        // a mere h2d.Object has no visual qualities to it
        // it's just a container by itself... even though added to the scene there's nothing to see...
        
        // a h2d.Text (like from the Hello World! sample)
        var text = new h2d.Text( hxd.res.DefaultFont.get(), s2d );
        text.text = "A simple h2d.Text";
        text.textColor = 0xFBC707;

        // a h2d.Bitmap (from coded tile, no resource used)
        var bitmap = new h2d.Bitmap( h2d.Tile.fromColor( 0xFF0000, 32, 32 ), s2d );

        // a h2d.Graphics
        var graphics = new h2d.Graphics( s2d );
        graphics.lineStyle( 2, 0xFFFFFF );
        graphics.beginFill( 0x33cc33 );
        graphics.drawCircle( 0, 0, 16 );
        graphics.beginFill( 0xff6666 );
        graphics.drawRect( 4, 4, 12, 12 );

        // an h2d.Anim object
        var k = 32;
        var tfa = []; // tiles for animation "tfa"
        tfa.push( h2d.Tile.fromColor( 0xFF0000, k, k ) );
        tfa.push( h2d.Tile.fromColor( 0xFFFF00, k, k ) );
        tfa.push( h2d.Tile.fromColor( 0x00FFFF, k, k ) );
        tfa.push( h2d.Tile.fromColor( 0x0000FF, k, k ) );
        tfa.push( h2d.Tile.fromColor( 0xFF00FF, k, k ) );
        var anim = new h2d.Anim( tfa, 2, s2d );

        // a h2d.Interactive (button-like) object
        var interactive = new h2d.Interactive( 100, 50, s2d );
        var col_default = 0xFF3366ff;
        var col_over    = 0xFF809fff;
        var col_push    = 0xFF33cc33;
        interactive.backgroundColor = col_default;
        interactive.onOver  = (e)->{ interactive.backgroundColor = col_over; };
        interactive.onOut   = (e)->{ interactive.backgroundColor = col_default; };
        interactive.onPush  = (e)->{ interactive.backgroundColor = col_push; };
        interactive.onClick = (e)->{ trace("Clicking button."); };

        // info texts
        explanationLabel( text,        "h2d.Text:" );
        explanationLabel( bitmap,      "h2d.Bitmap:" );
        explanationLabel( graphics,    "h2d.Graphics:" );
        explanationLabel( anim,        "h2d.Anim:" );
        explanationLabel( interactive, "h2d.Interactive:\n(click me)" );

        // scatter positions
        text       .setPosition( 200, 100 );
        bitmap     .setPosition( 200, 200 );
        graphics   .setPosition( 200, 300 );
        anim       .setPosition( 200, 400 );
        interactive.setPosition( 200, 500 );
    }
    
    function explanationLabel( obj:h2d.Object, explanation:String ) : h2d.Text {
        var t = new h2d.Text( hxd.res.DefaultFont.get(), obj );
        t.text = explanation;
        t.setPosition( -150, 0 );
        return t;
    }
}
```

---
## Object trees
Objects can be added to a scene directly (a `h2d.Scene`, for instance `s2d` when inside `hxd.App`) or be added to another `h2d.Object` creating an *object tree*. Object trees are regarded in the next section about [[Object trees]].
