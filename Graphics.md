# Drawing Graphics

Heaps offers an easy way to draw custom shapes and colors to the screen in the 2D context. Shapes can be filled with solid colors, gradients or custom bitmaps.

First you start by creating a custom graphics object.

```haxe
//Create a custom graphics object by passing a 2d scene reference.
var customGraphics = new h2d.Graphics(s2d);

//specify a color we want to draw with
customGraphics.beginFill(0xEA8220);
//Draw a rectangle at 10,10 that is 300 pixels wide and 200 pixels tall
customGraphics.drawRect(10, 10, 300, 300);
//End our fill
customGraphics.endFil();
```

The above code will produce the following
![Solid Box Drawn](img/h2d/drawbox.jpg)

You can also fill a graphics object with a loaded bitmap.  You use the 'beginTileFill()' method of the Graphics object as follows:

```haxe
var logo = hxd.Res.hxlogo.toTile();

var g = new h2d.Graphics(s2d);			
for(x in 0...4){
    for(y in 0...4){		
        g.beginTileFill(x*logo.width,y*logo.height,1,1,logo);		
        g.drawRect(x*logo.width, y*logo.height, logo.width, logo.height);			
    }
}
g.endFill();
```
The above code will produce the following series of tiled logos

![Haxe Logo](img/h2d/drawtilefill.jpg)

