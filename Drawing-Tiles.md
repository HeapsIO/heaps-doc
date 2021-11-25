# Drawing tiles

Tiles can be drawn with multiple approaches, some of which are going to be covered by following examples.

## h2d.Bitmap
`Bitmap` is a simple container to draw one `Tile`. It's the most basic type, but should be used with caution, as each `Bitmap` causes a draw call.

## h2d.TileGroup
`TileGroup` allows to batch-draw Tiles belonging to the same `Texture`, with the ability to apply individual tint, transform and alpha to each tile. This class can be used to draw static tiles, but is not very well suited for dynamic movement, since it should be cleared and refilled with data each time something should be altered.

### TileGroup hacking
With `@:privateAccess tileGroup.content` it's possible to access underlying `TileLayerContent` instance and do more fine-tuned geometry operations other than simple rectangles.

## h2d.SpriteBatch
`SpriteBatch` allows to batch-render Tiles belonging to the same `Texture`, but contrary to `TileGroup`, it updates batch data each frame and allows to control each `SpriteElement` individually. Note that by default SpriteBatch does not use scale and rotation of SpriteElements and it should be enabled with the `hasRotationScale` flag. The same applies to the `update` method of SpriteElement — it will be called only when the `hasUpdate` flag is set.

### BasicElement and updates
`h2d.SpriteBatch.BasicElement` is a good example of how the `update` method can be used. This class provides simple simulation of gravity, friction and velocity.

## Sample
This example draws tile layers from a [Tiled](http://www.mapeditor.org/) export file (json) using `TileGroup` batching, `Bitmap` for backdrop image and `SpriteBatch` for batch-rendering of moving sprites.

The example uses files _backdrop.png_, _bunnies.png_, _tiles.json_ and _tiles.png_. These files should be put in the resources (`/res`) folder. They are not included in samples directory.

```haxe
class Main extends hxd.App {
	static function main() {
		// embed the resources
		hxd.Res.initEmbed();
		new Main();
	}

	override private function init() {
		super.init();

		// Add backdrop Bitmap
		var bitmap = new h2d.Bitmap(hxd.Res.backdrop.toTile(), s2d);

		// parse Tiled json file
		var mapData:TiledMapData = haxe.Json.parse(hxd.Res.tiles_json.entry.getText());

		// get tile image (tiles.png) from resources
		var tileImage = hxd.Res.tiles_png.toTile();

		// create a TileGroup for fast tile rendering, attach to 2d scene
		var group = new h2d.TileGroup(tileImage, s2d);

		var tw = mapData.tilewidth;
		var th = mapData.tileheight;
		var mw = mapData.width;
		var mh = mapData.height;

		// make sub tiles from tile
		var tiles = [
			 for(y in 0 ... Std.int(tileImage.height / th))
			 for(x in 0 ... Std.int(tileImage.width / tw))
			 tileImage.sub(x * tw, y * th, tw, th)
		];

		// iterate on all layers
		for(layer in mapData.layers) {
			// iterate on x and y
			for(y in 0 ... mh) for (x in 0 ... mw) {
				// get the tile id at the current position
				var tid = layer.data[x + y * mw];
				if (tid != 0) { // skip transparent tiles
					// add a tile to the TileGroup
					group.add(x * tw, y * mapData.tilewidth, tiles[tid - 1]);
				}
			}
		}

		// Create raining bunnies with SpriteBatch
		var bunnies = hxd.Res.bunnies.toTile();
		var batch = new h2d.SpriteBatch(s2d);
		batch.hasRotationScale = true;
		batch.hasUpdate = true;
		// Note: Does not count as a bunnymark.
		for (i in 0...100) {
			var bunny = new Bunny(bunnies);
			batch.add(bunny);
			bunny.reset();
		}
	}
}

class Bunny extends h2d.SpriteBatch.SimpleElement {

	public function new(t:h2d.Tile) {
		super(t);
		gravity = 100;
	}

	public function reset() {
		y = -t.height;
		x = Math.random() * (batch.getScene().width - t.width);
		scale = 1 + (Math.random() - 0.5) * .1;
		vx = Math.random() * 60;
	}

	override function update(dt:Float) {
		super.update(dt);
		if (y > batch.getScene().height) reset();
		return true;
	}
}

// simple type definition for Tile map
typedef TiledMapData = { layers:Array<{ data:Array<Int>}>, tilewidth:Int, tileheight:Int, width:Int, height:Int };
```
