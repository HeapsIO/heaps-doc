# Object
Like shown previously in the introduction to give our Heaps application visual (2D) content we need classes that are build upon `h2d.Object`.

An **Object** (represented by [h2d.Object](https://heaps.io/api/h2d/Object.html)) is the base class of all 2D objects, so any thing that you can *see* on the screen. Therefore `h2d.Object` provides variables like a position (x,y), a scale (scaleX,scaleY), a rotation and methods to change them.
`h2d.Object`s can be any visual objects in the world of the game like for instance *the player, enemies, buildings*, but furthermore also buttons (`h2d.Interactive`) or a `h2d.Flow` that arranges UI elements and many more.

These are all `h2d` package classes (and sub-classes) that extend `h2d.Object` and can be used to create 2D content:
- Console
- Drawable
  - Anim, Bitmap, Particles, SpriteBatch
  - Graphics
    - Benchmark
  - Interactive
    - Slider
  - Text
    - HtmlText, TextInput
  - TileGroup
    - ScaleGrid
  - Video
- Flow
  - CheckBox, Dropdown
- Mask
  - KeyFrames
- Layers
  - CdbLevel, Scene, ZGroup
- ObjectFollower
- Loader

---
## Object trees
Objects can be added to a scene (here `h2d.Scene`) directly or be added to another `h2d.Object` creating an *object tree*.
The child objects will inherit the transformations of the parent object they have been added to. This means whenever the parent transforms (changing the position, rotating, making it transparent or invisible etc.) these transformations *also* apply to all the children that have been added to this parent object.

```haxe
var a = new h2d.Object();              // object not added yet
s2d.addChild(a);                       // now adding `a` to `s2d` (the currently active 2D scene)

var b = new h2d.Object( s2d );         // `b` is added directly via constructor to `s2d`
b.x = 100;

var c = new h2d.Object( b );           // `c` is added to `b` which has been added to a scene (`s2d`)
c.x = 10;                              // relative to parent `b`

trace( c.getAbsPos().getPosition() );  // the absolute x-position of `c` will be 110, because it "travels" along with its parent `b`
```

Note that `h2d.Object` is also documented in detail in the [API](https://heaps.io/api/h2d/Object.html)!

The following code should make it more clear why these trees of stacked objects are useful. As you will see, when moving the "clock" and the "NPC" all their *children* move with them.

```haxe
class Main extends hxd.App {
    var clock : h2d.Graphics;
    var npc : h2d.Graphics;
    var npc_moveX : Float = 0;
    var npc_moveY : Float = 0;
    static function main() {new Main();}
    public function new() {super();}
    override function init() {
        
        clock = new h2d.Graphics( s2d );
        clock.beginFill( 0x505050 );
        clock.drawCircle( 0, 0, 90 );
        clock.endFill();
        clock.setPosition( 100, 100 );

        var face = new h2d.Graphics( clock );
        face.beginFill( 0xFFFFFF );
        face.drawCircle( 0, 0, 85 );
        face.endFill();

        var tile = h2d.Tile.fromColor( 0x0, 75, 12 ); tile.dy = -6;
        var handHours   = new h2d.Bitmap( tile, clock ); handHours.rotation = 1.5*Math.PI;
        var tile = h2d.Tile.fromColor( 0x0, 80,  8 ); tile.dy = -4;
        var handMinutes = new h2d.Bitmap( tile, clock );
        // defining a timer to make the clock's hands tick
        var t = new haxe.Timer(1000);
        t.run = ()->{
            handHours.rotate( Math.PI/300 ); // actually would be 3600 for real hours
            handMinutes.rotate(  Math.PI/60 );
        };

        npc = new h2d.Graphics( s2d );
        npc.setPosition( 300, 300 );

        // NPC's face
        npc.beginFill( 0xFFFF00 );
        npc.drawCircle( 0, 0, 40 );
        npc.endFill();
        // first eye
        npc.beginFill( 0x0 );
        npc.drawCircle( -20, 0, 7 );
        npc.endFill();
        // second eye
        npc.beginFill( 0x0 );
        npc.drawCircle( 20, 0, 7 );
        npc.endFill();
        // a happy mouth
        npc.beginFill( 0x0 );
        npc.drawPie( 0, 10, 15, 0*Math.PI, 1*Math.PI );
        npc.endFill();
        // and some eye sparkle
        npc.beginFill( 0xFFFFFF );
        npc.drawCircle( -20, -5, 3 );
        npc.endFill();
        npc.beginFill( 0xFFFFFF );
        npc.drawCircle( 20, -5, 3 );
        npc.endFill();

        var healthbar = new h2d.Graphics( npc );
        healthbar.setPosition(-50,-70);
        healthbar.beginFill( 0xFF0000 );
        healthbar.drawRect( 0, 0, 100, 20 );
        healthbar.beginFill( 0x00FF00 );
        healthbar.drawRect( 0, 0,  70, 20 );
        healthbar.endFill();

        var healthbar_label = new h2d.Text( hxd.res.DefaultFont.get(), healthbar );
        healthbar_label.text = "NPC\n70 lifepoints left";
        healthbar_label.setPosition( 0, -32 );

        var npc_moveVectorLabel =  new h2d.Text( hxd.res.DefaultFont.get(), npc );
        npc_moveVectorLabel.setPosition(-50,50);
        // defining a timer to change the NPC's movement vector
        var t = new haxe.Timer(5*1000); // change moving direction every 5 seconds
        t.run = ()->{
            npc_moveX = Math.round( ((-1+(Math.random()*2))) * 100) / 100;
            npc_moveY = Math.round( ((-1+(Math.random()*2))) * 100) / 100;
            npc_moveVectorLabel.text = 'move vector (x: ${npc_moveX}, y: ${npc_moveY})';
        };
    }
    override function update(dt:Float) {
        super.update(dt);
        // clock movement
        clock.x += 0.2;
        if( clock.x>400 )
            clock.x = 100;
        // npc movement
        npc.x += npc_moveX;
        npc.y += npc_moveY;
        // npc never leaves scene
        if( npc.x < 0 || npc.y < 0 || npc.x > s2d.width || npc.y > s2d.height )
            npc.setPosition( 300, 300 );
    }
}
```

Side note: Here by using `haxe.Timer` we rely on simple [Haxe API](https://api.haxe.org/) to provide another way besides the `update` method to make changes.