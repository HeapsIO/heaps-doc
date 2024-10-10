# Breakout

This is a very basic template for a breakout clone. The principle is inspired by the [Atari Breakout video game from 1976](https://en.wikipedia.org/wiki/Breakout_(video_game)).

## Screenshot

![breakout_clone](https://user-images.githubusercontent.com/88530062/174796074-f35166f2-61fd-489d-ba97-2ca9689d75ea.png)

## Code

```haxe

class BreakoutGame extends hxd.App { //BreakoutGame
    var player : h2d.Bitmap;
    var bricks : Array<h2d.Object> = [];
    var ball : h2d.Graphics;
    var ball_moveX : Float; // controls where the ball is moving along the x-axis
    var ball_moveY : Float; // same but for y-axis
    static function main() {new BreakoutGame();}
    override function init() {

        // create player
        var tile = h2d.Tile.fromColor( 0xFFFF00, 200, 32 ).center();
        player = new h2d.Bitmap( tile , s2d );

        // create ball
        ball = new h2d.Graphics( s2d );
        ball.beginFill( 0xFFFFFF );
        ball.drawCircle( 0, 0, 8 );
        ball.setPosition( player.x, player.y-32 );

        reset_player_position();
        fill_board_with_bricks();

        ball_moveX =  8; // 8 is really fast!
        ball_moveY = -8;

        // info text for the game
        var t = new h2d.Text( hxd.res.DefaultFont.get(), s2d );
        t.text = "Use <A> and <D> or the arrow keys <LEFT> and <RIGHT> to move.\nPress <R> or <BACKSPACE> to reset the board.";
    }

    function reset_player_position() {
        player.setPosition( s2d.width/2, s2d.height-40 );
    }

    function fill_board_with_bricks() {
        bricks = []; // empty bricks array
        var addBrick = ()->{
            var brick = new h2d.Bitmap( h2d.Tile.fromColor( 0x00FFFF, 48, 16 ), s2d );
            bricks.push( brick );
            return brick;
        };
        for( yy in 0...20 )
            for( xx in 0...40 )
                addBrick().setPosition( (xx*48)+(xx*2), 32+(yy*16)+(yy*2) ); // (xx*2) and (yy*2) will make small gaps of 2 pixels
    }

    override function update(dt:Float) {

        // player movement
        var player_speed = 8;
        if( hxd.Key.isDown( hxd.Key.LEFT ) || hxd.Key.isDown( hxd.Key.A ) )
            player.x -= player_speed;
        if( hxd.Key.isDown( hxd.Key.RIGHT ) || hxd.Key.isDown( hxd.Key.D ) )
            player.x += player_speed;

        // ball movement
        ball.x += ball_moveX;
        ball.y += ball_moveY;
        // ball bounces from s2d's "walls"
        if( ball.x + ball_moveX < 0 || ball.x + ball_moveX > s2d.width )
            ball_moveX = ball_moveX * (-1);
        if( ball.y + ball_moveY < 0 || ball.y + ball_moveY > s2d.height )
            ball_moveY = ball_moveY * (-1);

        // check collisions
        // 1. ball with bricks
        for( br in bricks )
            if( ballCanBounceAgainst( br ) ){
                br.remove();              // destroy brick
                bricks.remove( br );      // also remove from bricks array! otherwise it'll keep the reference!
                trace('Bricks left: ${bricks.length}');
            }
        // 2. ball with player
        ballCanBounceAgainst( player );

        // damn my code.......
        // in special case the ball escapes the visible part of the sceen
        if( ball.x < 0 || ball.x > s2d.width || ball.y < 0 || ball.y > s2d.height ){
            ball.setPosition( player.x, player.y-40 );
            trace("Damn the programmer! Ball escaped and had to be reset!");
        }

        if( bricks.length == 0 )
            trace("You won the game!!");

        // reset board
        if( hxd.Key.isPressed( hxd.Key.R ) || hxd.Key.isPressed( hxd.Key.BACKSPACE ) ){
            reset_player_position();
            fill_board_with_bricks();
        }
    }

    function ballCanBounceAgainst( obj:h2d.Object ) : Bool {
        var doesCollide = false;
        var ball_bounds = ball.getBounds();
        ball_bounds.offset( ball_moveX, ball_moveY ); // where the ball will be one frame ahead in the future. This will prevent glitches...
        // ...(for instance the ball might "ghost pass" through several bricks!)
        var other_bounds = obj.getBounds();
        if( ball_bounds.intersects( other_bounds ) ){
            doesCollide = true;
            if( ball.x > other_bounds.xMin && ball.x < other_bounds.xMax ) // bounce from top or bottom
                ball_moveY = ball_moveY * (-1);
            if( ball.y > other_bounds.yMin && ball.y < other_bounds.yMax ) // bounce from left or right
                ball_moveX = ball_moveX * (-1);
        }
        return doesCollide;
    }
}
```

## hxml-file sample (here for HashLink)

```
-lib heaps
-lib hlsdl
-main BreakoutGame
-hl breakout.hl
-D windowSize=800x600
```