# Layers

When using any `h2d.Scene` (`s2d` when inside `hxd.App`) you have all features of the `h2d.Layers` class already available. This is because `h2d.Scene` `extends h2d.Layers`.

**Layers** give more control over what is *rendered* first. You can control what is in front and what is in the back. In the demo below clouds are supposed to be **in front and not in the back behind a house or meadow!**.

Basically you have two features here:

1. The first is you can place `h2d.Object`s at specific layers by `s2d.addChildAt( myobj, MY_LAYER )`. `0` will be the layer the most in the background, while each higher layer is rendered over/above the precedent layer. Usually you predefine your layer indices in some way for instance by `final MY_LAYER_NAME : Int = 23;` (see in the sample below) so you know what each integer (`Int` value) stands for.

2. The second feature is that the layer class also allows to control objects individually by `.over()` and `.under()`. Finally you can benefit from the `ysort()` method to order a layer. *In the sample below this will prevent housings to overlap in a weird way. A house farther away from our viewpoint should be rendered behind houses that are closer!*

## Sample

![layers_houses_clouds](https://user-images.githubusercontent.com/88530062/174480849-d776e6d4-18c3-4221-98ac-aeec4457ef3a.png)

Note that the clouds are on top of all other objects. The green soil stains (meadows) are the most beneath (because they use layer `0`). The houses regard their y-Position: When another house has a lower `y` value it is rendered behind its competitor (and not awkwardly in front)!

```haxe

class LayersDemo extends hxd.App {
    var clouds : Array<h2d.Object> = [];
    var LAYER_FOR_SOIL : Int = 0;
    var LAYER_FOR_BUILDINGS : Int = 1;
    var LAYER_FOR_SKY : Int = 2;
    static function main() {new LayersDemo();}
    function new() {super();}
    override function init() {
        engine.backgroundColor = 0xFF33cc33; // game's background color is now green
        for( i in 0...40 )
            add_house();
        for( i in 0...40 )
            add_cloud();
        for( i in 0...20 )
            add_soil_stain();
        // button to add houses
        var addHouseButton = new h2d.Interactive( 200, 32, s2d ); addHouseButton.backgroundColor = 0xFF0000FF;
        addHouseButton.onClick = (e)->{ var h = add_house(); trace('New house at (${h.x}|${h.y})'); };
        var t = new h2d.Text( hxd.res.DefaultFont.get(), addHouseButton );
        t.text = "Press here to add a house";
    }
    function add_house() : h2d.Object {
        var g = new h2d.Graphics();
        g.beginFill( 0xFFFFFF );
        g.drawRect( -20, 0, 40, 24 );
        // a window
        g.beginFill( 0x6699ff );
        g.drawRect( -8, 8, 8, 8 );
        g.beginFill( 0x6699ff );
        g.drawRect(  2, 8, 8, 8 );
        // roof
        g.beginFill( 0xFF0000 );
        g.addVertex( -20,   0, 1, 0, 0, 1 );
        g.addVertex(   0, -16, 1, 0, 0, 1 );
        g.addVertex(  20,   0, 1, 0, 0, 1 );
        g.addVertex( -20,   0, 1, 0, 0, 1 );
        g.endFill();
        set_to_random_postion( g );
        s2d.addChildAt( g, LAYER_FOR_BUILDINGS );
        return g; // allows to receive the reference to the created object...
    }
    function add_cloud() {
        var g = new h2d.Graphics();
        // Haxe is very mighty when it comes to functions:
        var add_row_of_bubbles = (maxAmountAdd:Int, yLevel:Float ) -> {
            var bubbles : Int = 3 + Math.round( hxd.Math.random( maxAmountAdd ) );
            var colors : haxe.ds.Vector<Int> = new haxe.ds.Vector(3);
            colors[0] = 0xFFFFFF; colors[1] = 0xf2f2f2; colors[2] = 0xe6e6e6;
            var i : Int = Math.round( hxd.Math.random( colors.length-1) );
            g.beginFill( colors[i] );
            for( i in 0...bubbles )
                g.drawCircle( -16+hxd.Math.random(32), 0+yLevel, 4 + hxd.Math.random(8) );
        }
        add_row_of_bubbles( 8, -8 );  // upper level of bubbles
        add_row_of_bubbles( 24,  0 ); // lower level of bubbles
        set_to_random_postion( g );
        clouds.push( g );
        s2d.addChildAt( g, LAYER_FOR_SKY );
    }
    function add_soil_stain() {
        var g = new h2d.Graphics();
        g.beginFill( 0x84e184 ); // lighter green
        g.drawEllipse( 0, 0, 32 + hxd.Math.random(64), 8 + hxd.Math.random(8) );
        set_to_random_postion( g );
        s2d.addChildAt( g, LAYER_FOR_SOIL );
    }
    function set_to_random_postion( obj:h2d.Object ) {
        obj.setPosition( hxd.Math.random( s2d.width ), hxd.Math.random( s2d.height ) );
    }
    override function update(dt:Float) {
        for( c in clouds ){
            c.x += 0.1;
            if( c.x > s2d.width )
                c.x = 0;
        }
        s2d.ysort( LAYER_FOR_BUILDINGS ); // buildings rendered in correct order!
    }
}
```

Note that all we have to use is `s2d.addChildAt( myobj, MY_LAYER )` instead of just `s2d.addChild( myobj )` and finally we use `s2d.ysort( LAYER_FOR_BUILDINGS )` to arrange all houses.