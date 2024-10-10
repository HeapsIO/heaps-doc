# Rogue

This is a simple template to make a game inspired by [Rogue from 1980](https://en.wikipedia.org/wiki/Rogue_(video_game)), thus to make a [*roguelike*](https://en.wikipedia.org/wiki/Roguelike).

## Screenshot

![roguelike](https://user-images.githubusercontent.com/88530062/174960136-c4ed996e-7533-4876-8872-2164e6ec0109.png)

## Code

```haxe
class Roguelike extends hxd.App {
    var player : h2d.Object;
    final fieldSize = 32;
    var levelString : String = "

                                                    ###################
               ###########################          #                 #
               #                         #          #            A    #
               #                         ############                 #
               #                                                      #
               #            @            ############                 #
               #                         #          #                 #
               #                         #          #      C          #
               #                         #          ###################
               #   E                     #
               #                         #
               ###########################
    ";
    var walls : Array<h2d.Object> = [];
    var howToPlayInfo : h2d.Object;
    static function main() {new Roguelike();}
    override function init() {
        var t = new h2d.Text( hxd.res.DefaultFont.get(), s2d );
        t.text = "Use the arrow keys\nor w-a-s-d to move around."; t.scale(2);
        howToPlayInfo = t;

        // setting up the level from a string
        var x = 0;
        var y = 0;
        for( i in 0...levelString.length ){
            var char = levelString.charAt( i );
            if( char=="\n" ){
                y++;
                x=0;
            }
            else
                if( char!=" ")
                    characterToGameObject( char, x, y );
            x++;
            
        }

        // setting camera
        s2d.camera.anchorX = 0.5;
        s2d.camera.anchorY = 0.5;

        // placing how-to-play info correctly (after player has been placed)
        var cam = s2d.camera;
        howToPlayInfo.setPosition( player.x - (cam.viewportWidth/2), player.y - (cam.viewportHeight/2) );
    }

    function characterToGameObject( character:String, x:Float, y:Float ){
        if( character!=" " ){ // no empty objects
            var o = new h2d.Object( s2d );
            var t = new h2d.Text( hxd.res.DefaultFont.get(), o );
            t.text = character; t.scale( 2 );
            o.setPosition( x *fieldSize, y *fieldSize );
            // creating the "real" logical object
            switch ( character ){
                case "@": // the player
                    player = o;
                    // basic camera
                    s2d.camera.follow = player;
                case "#": // walls
                    walls.push( o );
                default:
                    trace('The character <$character> hasn\'t been assigned to a game object.'); // doesn't work right... but good enough
            }
        }
    }

    var dt_counter : Float = 0;

    override function update(dt:Float) {
        dt_counter += dt;
        if( dt_counter > 0.1 ){ // 10 times per second
            // player movement
            if( hxd.Key.isDown( hxd.Key.UP   )  || hxd.Key.isDown( hxd.Key.W ) )
                moveIfPositionIsWallFree( player, player.x, player.y - fieldSize ); // okay the code's a bit clunky I admit that!
            if( hxd.Key.isDown( hxd.Key.DOWN )  || hxd.Key.isDown( hxd.Key.S ) )
                moveIfPositionIsWallFree( player, player.x, player.y + fieldSize );
            if( hxd.Key.isDown( hxd.Key.LEFT )  || hxd.Key.isDown( hxd.Key.A ) )
                moveIfPositionIsWallFree( player, player.x - fieldSize, player.y );
            if( hxd.Key.isDown( hxd.Key.RIGHT ) || hxd.Key.isDown( hxd.Key.D ) )
                moveIfPositionIsWallFree( player, player.x + fieldSize, player.y );

            dt_counter -= 0.1; // reset
            // could also just do `dt_counter = 0;` but is less fine
        }
    }

    function moveIfPositionIsWallFree( obj:h2d.Object, x:Float, y:Float ) {
        if( checkIfPositionIsWallFree(x,y) )
            obj.setPosition(x,y);
    }

    function checkIfPositionIsWallFree( x:Float, y:Float ) : Bool {
        var free = true;
        for( w in walls ){
            if( w.x == x && w.y == y )
                free = false;
        }
        return free;
    }
}
```

## hxml-file sample when using HashLink
```
-lib heaps
-lib hlsdl
-main Roguelike
-hl roguelike.hl
```

## Further hints

- The user could even create their own levels in a simple text file editor. You could then get the string by `sys.io.File.getContent( "path/to/file.txt" )` and use the string as a `levelString` input.

- You can also close the gaps in the walls (the gaps between the letters). You can scale up each `h2d.Text` object by a higher factor (`3` for instance). However to completely close all gaps you may have to split up `fieldSize` into two values (like `fieldW` and `fieldH`), depending on the letter size of the Font you are using.
