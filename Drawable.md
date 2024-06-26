# Drawable

H2D classes that can display something on screen usually extend the [`h2d.Drawable`](https://github.com/ncannasse/heaps/blob/master/h2d/Drawable.hx) class.

Each Drawable (including [`h2d.Bitmap`](https://github.com/ncannasse/heaps/blob/master/h2d/Bitmap.hx)) has several properties that can be manipulated:

* `alpha` : this will change the amount of transparency your drawable is displayed with. For instance a value of 0.5 will display a Tile with 50% opacity.
* `color` : color is the color multiplier of the drawable. You can access its individual channels with (r,g,b,a) components. It is initialy set to white (all components are set to 1.). Setting for instance the (r,g,b) components to 0.5 will make the tile appear darker.
* `blendMode` : the blend mode tells how the drawable gets composited with the background color when it is drawn on the screen. The background color refers to the screen content at the time the sprite is being drawn. This can be another sprite content if it was drawn before at the same position. The following blend mode values are available :  
	
    * `Alpha` (default) : the drawable blends itself with the background using its alpha value. An opaque pixel will erase the background, a fully transparent one will be ignored.
    * `None` : this disable the blending with the background. Alpha channel is ignored and the color is written as-is. This offers the best display performances for large backgrounds that have nothing displayed behind them
    * `Add` : the drawable color will be added to the background, useful for creating explosions effects or particles for instance.
    * `SoftAdd` : similar to Add but will prevent over saturation
    * `Multiply` : the sprite color is multiplied by the background color
    * `Erase` : the sprite color is subtracted to the background color
   
* `filter` : when a sprite is scaled (upscaled or downscaled), by default Heaps will use the nearest pixel in the Tile to display it. This will create a nice pixelated effect for some games, but might not looks good on your game. You can try to set the filter value to true, which will enable bilinear filtering on the sprite, making it looks less sharp and more smooth/blurry.
* `shaders` : each `Drawable` can have shaders added to modify their display. Shaders are introduced [here](https://github.com/HeapsIO/heaps/wiki/H2D-Shaders).

`Drawable` instances have other properties which can be discovered by visiting the [`h2d.Drawable`](https://github.com/ncannasse/heaps/blob/master/h2d/Drawable.hx) API section.

## Sample

A collection of different objects that all inherit from `h2d.Drawable`:

### Screenshot

![allKindsOfObjects_demo](https://user-images.githubusercontent.com/88530062/174468670-4ea6ddd8-39b7-4491-8a70-fbb49f240594.png)

### Code

```haxe
class SomeDrawables extends hxd.App {
    static function main() {new SomeDrawables();}
    override function init() {

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
