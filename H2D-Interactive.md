# H2D Interactive

## Sample for a moving Interactive

```
class H2DInteractiveDemo extends hxd.App
{
    static function main(){ new H2DInteractiveDemo(); }

    var b:h2d.Bitmap;

    override function init() 
    {
        b = new h2d.Bitmap( h2d.Tile.fromColor(0xff0000, 60, 60), s2d);
        b.x = 100;
        b.y = 100;
        
        b.tile = b.tile.center();
        b.rotation = Math.PI / 4;
        
        var i = new h2d.Interactive(b.tile.width, b.tile.height, b);
        
        i.x = -0.5 * b.tile.width;
        i.y = -0.5 * b.tile.height;

        i.onOver = function(_) b.alpha = 0.5;
        i.onOut  = function(_) b.alpha = 1.0;

	// info text
	var t = new h2d.Text( hxd.res.DefaultFont.get(), s2d );
	t.text = "Hover over the square with the mouse.";
    }

    override function update(dt:Float)
    {
        b.rotation += 0.01;
    }
}

```