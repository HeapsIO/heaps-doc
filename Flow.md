# Flow

The [`h2d.Flow` class](https://heaps.io/api/h2d/Flow.html) allows to properly *arrange* [child objects](Object-trees) (`h2d.Object`).

## A first glance at Flows

### Demo 1

#### Screenshot 1


![flows_fastdemo](https://user-images.githubusercontent.com/88530062/175258407-4ce8b93a-618c-4137-90c6-acc27d4b31d0.png)


As you can see every child is placed without overlapping each other. The default layout is `Horizontal` (`h2d.Flow.FlowLayout.Horizontal`), thus the elements are placed next to each other and that horizontally.

#### Code 1

```haxe
class FlowsDemo extends hxd.App {
    static function main() {new FlowsDemo();}
    override function init() {
        
        // the mere container
        var flow = new h2d.Flow( s2d );
        flow.setPosition( 50, 50 );

        // setting the flow design
        // by default will be:
        //flow.layout = h2d.Flow.FlowLayout.Horizontal;

        // these objects below get added to the flow

        var t = new h2d.Text( hxd.res.DefaultFont.get(), flow );
        t.text = "Some text.";

        var tile = h2d.Tile.fromColor( 0x0000FF, 96, 32 );
        var b = new h2d.Bitmap( tile, flow );

        var t = new h2d.Text( hxd.res.DefaultFont.get(), flow );
        t.text = "A bigger text.";
        t.scale(3);

        var ia = new h2d.Interactive( 50, 50, flow );
        ia.backgroundColor = 0xFF808080;
        ia.onClick = (e)->{trace("Click!");};

        // red line to draw the flow borders
        var t = new h2d.Text( hxd.res.DefaultFont.get(), s2d );
        t.text = "Red-lined rectangle represents the h2d.Flow object.";
        var g = new h2d.Graphics( s2d );
        g.lineStyle( 1, 0xFF0000 );
        var bounds = flow.getBounds();
        g.drawRect( bounds.x, bounds.y, bounds.width, bounds.height );
    }
}
```

### Demo 2

The second sample code will give the `h2d.Interactive` the ability to change the properties of the Flow. Each time you **press the button "click me!"** the Flow content changes and a `trace()` prints the currently active Flow setting to the console.

#### Screenshot 2

![flows_fastdemo_2](https://user-images.githubusercontent.com/88530062/175290399-6881f5f1-6f3d-4983-989f-9ceaadcfacaf.png)


#### Code 2

```haxe
class FlowsDemo2 extends hxd.App {
    static function main() {new FlowsDemo2();}
    override function init() {
        
        // our Flow object
        var flow = new h2d.Flow( s2d );
        flow.setPosition( 50, 50 );

        // these objects below get added to the flow

        var t = new h2d.Text( hxd.res.DefaultFont.get(), flow );
        t.text = "Some text.";

        var tile = h2d.Tile.fromColor( 0x0000FF, 96, 32 );
        var b = new h2d.Bitmap( tile, flow );

        var t = new h2d.Text( hxd.res.DefaultFont.get(), flow );
        t.text = "A bigger text.";
        t.scale(3);

        // red line to draw the flow borders
        var t = new h2d.Text( hxd.res.DefaultFont.get(), s2d );
        t.text = "Red-lined rectangle represents the h2d.Flow object.";
        var g = new h2d.Graphics( s2d );
        var draw_flow_borders = ()->{
            g.clear();
            g.lineStyle( 1, 0xFF0000 );
            var bounds = flow.getBounds();
            g.drawRect( bounds.x, bounds.y, bounds.width, bounds.height );
        };

        // this interactive will also be part of the Flow
        // but also serves as button to change the Flow's properties

        var ia = new h2d.Interactive( 50, 50, flow );
        ia.backgroundColor = 0xFF00cc66;
        ia.onClick = (e)->{
            // random flow layout
            var a = h2d.Flow.FlowLayout.createAll();
            flow.layout = a[ Math.round( hxd.Math.random( a.length-1 ) ) ];
            // random flow align
            var a = h2d.Flow.FlowAlign.createAll();
            flow.horizontalAlign = a[ Math.round( hxd.Math.random( a.length-1 ) ) ];
            flow.verticalAlign   = a[ Math.round( hxd.Math.random( a.length-1 ) ) ];
            // random spacing
            var s = Math.floor( hxd.Math.random(48) );
            flow.horizontalSpacing = s;
            var s = Math.floor( hxd.Math.random(48) );
            flow.verticalSpacing   = s;
            
            trace("Flow's propterties have been changed");
            trace( "\nlayout = "+flow.layout, "\nhorizontalAlign = "+flow.horizontalAlign, "\nverticalAlign = "+ flow.verticalAlign, "\nhorizontalSpacing = "+flow.horizontalSpacing, "\nverticalSpacing = "+flow.verticalSpacing );

            draw_flow_borders();
        };
        var t = new h2d.Text( hxd.res.DefaultFont.get(), ia );
        t.text = "click me!";

        draw_flow_borders();
    }
}
```

## More samples

There are other samples you can look at and learn from:

- Another interactive [example to understand the use of Flows](https://github.com/Beeblerox/Simplest-Heaps-Examples/tree/master/20_heaps_flow)
- The most detailed sample at this moment can be found [in our repository](https://github.com/HeapsIO/heaps/blob/master/samples/Flows.hx) (which you'll also find on your local machine: `HaxeToolkit\haxe\lib\heaps\git\samples\Flows.hx`)