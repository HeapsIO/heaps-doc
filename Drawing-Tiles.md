# Drawing Tiles

Tiles can be drawn with multiple approaches, some of which will be discussed below.

## h2d.Bitmap

`Bitmap` is a simple container to draw one `Tile`. It's the most basic type, but it should be used with caution, as each Bitmap creates a draw call.

## h2d.TileGroup

`TileGroup` allows you to batch-draw Tiles that share the same `Texture`. You can adjust the tint, position, and transparency of each tile. This class can be used to draw static tiles, but it's not well suited for dynamic movement because it must be cleared and refilled when changes are made.

> **Pro Tip:**
> With `@:privateAccess tileGroup.content`, you can access the underlying `TileLayerContent` instance, and perform advanced geometry operations beyond simple rectangles.

## h2d.SpriteBatch

`SpriteBatch` allows you to batch-render Tiles that share the same `Texture`. Unlike `TileGroup`, it updates data every frame and lets you control each `SpriteElement` individually. Note that, by default, SpriteBatch does not handle the scale or rotation of SpriteElements; it must be enabled with the `hasRotationScale` flag. The same applies to the `update()` method of SpriteElement — it will only be called when the `hasUpdate` flag is set.

`h2d.SpriteBatch.BasicElement` demonstrates how to use the `update()` method. This class also provides a basic simulation of gravity, friction, and velocity.

## Sample

This example demonstrates how to draw tile layers from a [Tiled](https://www.mapeditor.org/) export file in JSON format. It uses `TileGroup` for static terrain, `Bitmap` for a backdrop image, and `SpriteBatch` for rendering moving sprites.

To recreate this example, place the following files — _backdrop.png_, _bunnies.png_, _tiles.png_, and _tiles.json_ — in the resources (`/res`) folder. These files are not included in the samples directory.

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
		var tileImage  = hxd.Res.tiles_png.toTile();

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
					group.add(x * tw, y * th, tiles[tid - 1]);
				}
			}
		}

		// Create raining bunnies with SpriteBatch
		var bunnies = hxd.Res.bunnies.toTile();
		var batch = new h2d.SpriteBatch(bunnies, s2d);
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

class Bunny extends h2d.SpriteBatch.BasicElement {

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
