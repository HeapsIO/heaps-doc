# Flow

The [`h2d.Flow` class](https://heaps.io/api/h2d/Flow.html) allows to properly *arrange* [child objects](Object trees) (`h2d.Object`).

We have three samples here:
1. A simple first glance at Flow (see below)
2. A [basic example to understand the use](https://github.com/Beeblerox/Simplest-Heaps-Examples/tree/master/20_heaps_flow)
3. A [detailed sample in our repository](https://github.com/HeapsIO/heaps/blob/master/samples/Flows.hx) (which you'll also find on your local machine: `HaxeToolkit\haxe\lib\heaps\git\samples\Flows.hx`)

## A first glance at Flows

### Screenshot


![flows_fastdemo](https://user-images.githubusercontent.com/88530062/175258407-4ce8b93a-618c-4137-90c6-acc27d4b31d0.png)


As you can see every child is placed without overlapping each other when using the flow layout `Horizontal` here (`h2d.Flow.FlowLayout.Horizontal`).

### Code

```haxe
class Flows extends hxd.App {
    static function main() {new Flows();}
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