# Drawing tiles

This example draws tile layers from a [Tiled](http://www.mapeditor.org/) file (json), it can be used for fast tile rendering.

The example uses a _tiles.json_ and _tiles.png_ file, that should be put in the resources (`/res`) folder.

```haxe
class Main extends hxd.App {
	static function main() {
		// embed the resources
		hxd.Res.initEmbed();
		new Main();
	}
	
	override private function init() {
		super.init();
		
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
					group.add(x * tw, y * mapData.tilewidth, tiles[tid - 1]);
				}
			}
		}
	}
}

// simple type definition for Tile map 
typedef TiledMapData = { layers:Array<{ data:Array<Int>}>, tilewidth:Int, tileheight:Int, width:Int, height:Int };
```
