# Platform game

A template for a very basic [platform game](https://en.wikipedia.org/wiki/Platform_game).

## Screenshot

![platform_game_sample](https://user-images.githubusercontent.com/88530062/174939197-b3962d74-38c7-4bd3-8b43-d78f73347102.png)

## Code

```haxe
class PlatformGame extends hxd.App {
    var groundLevel : Float;
    var player : h2d.Object;
    var player_vspeed : Float = 0; // the vertical speed of the player
    var platforms : Array<h2d.Object> = [];
    var trampolines : Array<h2d.Object> = [];
    static function main() {new PlatformGame();}
    override function init() {
        var howtoplayinfo = new h2d.Text( hxd.res.DefaultFont.get(), s2d );
        howtoplayinfo.text = "Use arrow keys or w-a-s-d to move (also <SPACE> to jump)";

        // level ground / earth
        groundLevel = s2d.height-64;
        var ground = new h2d.Graphics( s2d );
        ground.beginFill( 0x808080 );
        ground.drawRect( 0, groundLevel, s2d.width, s2d.height );
        
        // creating the player
        player = new h2d.Object(s2d); // a mere container (this position we'll use as player's feet position)
        // player's body = bitmap (so we can use a real image file later here)
        var tile = h2d.Tile.fromColor( 0x0000FF, 12, 24 );
        tile.dx -= 6;
        var b = new h2d.Bitmap( tile, player );
        b.setPosition( 0, -24 );
        // player's head
        var g = new h2d.Graphics( player );
        g.beginFill( 0x0000FF );
        g.drawCircle( 0, 0, 8 );
        g.setPosition( 0, -32 );

        player.setPosition( s2d.width/2, 0 );

        // creating some platforms
        for( i in 0...100 ){
            var tile = h2d.Tile.fromColor( 0xA0A0A0, Math.round( 64+hxd.Math.random(320) ), Math.round( 16 + hxd.Math.random(32) ) );
            var p = new h2d.Bitmap( tile, s2d );
            p.setPosition( -8000 + hxd.Math.random(16000), 50 + hxd.Math.random( s2d.height-50-48-32 ) );
            platforms.push( p );
        }

        // create trampolines
        for( i in 0...100 ){
            var t = new h2d.Graphics( s2d );
            t.beginFill( 0xFFFF00 );
            t.addVertex( -16,  0, 1, 1, 1, 1 );
            t.addVertex(   0,-16, 1, 1, 1, 1 );
            t.addVertex(  16,  0, 1, 1, 1, 1 );
            t.addVertex( -16,  0, 1, 1, 1, 1 );
            t.setPosition( -8000 + hxd.Math.random(16000), groundLevel );
            trampolines.push( t );
        }
    }
    override function update(dt:Float) {

        // gravity force (very basic...)
        player.y += player_vspeed;
        // when on earth ground
        if( player.y < groundLevel ){
            player_vspeed+=0.2;
        }
        else {
            player_vspeed = 0;
            player.y = groundLevel;
        }
        // when on platform
        var isOnPlatform : Bool = false;
        for( p in platforms ){
            if( player.getBounds().intersects( p.getBounds() ) && player.y < p.y+16 ){
                player.y = p.y;
                player_vspeed = 0;
                isOnPlatform = true;
            }
        }

        // can jump when on ground
        if( hxd.Key.isPressed( hxd.Key.UP ) || hxd.Key.isPressed( hxd.Key.W ) || hxd.Key.isPressed( hxd.Key.SPACE ) )
        //if( player_vspeed == 0 && player.y == groundLevel ){ 
        if( player.y == groundLevel || isOnPlatform ){
            player_vspeed = -8;
        }
        // move left & right
        if( hxd.Key.isDown( hxd.Key.LEFT ) || hxd.Key.isDown( hxd.Key.A ) ){
            player.x -= 2;
            for( o in platforms )
                o.x += 2;
            for( o in trampolines )
                o.x += 2;
        }
        if( hxd.Key.isDown( hxd.Key.RIGHT ) || hxd.Key.isDown( hxd.Key.D ) ){
            player.x += 2;
            for( o in platforms )
                o.x -= 2;
            for( o in trampolines )
                o.x -= 2;
        }
        // trampoline effects
        for( t in trampolines )
            if( player.getBounds().intersects( t.getBounds() ) )
                player_vspeed = -15;
        // player can't escape scene
        if( player.y<0 )
            player.y=0;
        if( player.y>groundLevel )
            player.y=groundLevel;
        var x_offset_to_keep = 400; // keep a gap between player and scene's horizontal borders
        if( player.x<0+x_offset_to_keep )
            player.x=0+x_offset_to_keep;
        if( player.x>s2d.width-x_offset_to_keep )
            player.x=s2d.width-x_offset_to_keep;
        
        //trace(player.x, player.y, player_vspeed);
    }
} 
```

## A basic hxml-file proposal (for HashLink)

```
-lib heaps
-lib hlsdl
-main PlatformGame
-hl platform_game.hl
-D windowSize=1280x720
```