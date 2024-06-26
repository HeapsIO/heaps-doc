# Object trees
Objects can be added to a scene (here `h2d.Scene`) directly or be added to another `h2d.Object` creating an *object tree*.
The child objects will inherit the transformations of the parent object they have been added to. This means whenever the parent transforms (changing the **position**, **rotation**, **alpha**=transparency or turning it **invisible** etc.) these transformations *also* apply to all the children that have been added to this parent object.

## Sample 1

### Preview

![object_trees_1](https://user-images.githubusercontent.com/88530062/174791432-018842d2-2eab-4e44-8d2b-0cded089610a.png)


### Code

```haxe
class ObjectTrees extends hxd.App { // ObjectTrees
    var parent_obj  : h2d.Object;
    var a_child_obj : h2d.Object;
    static function main() {new ObjectTrees();}
    override function init() {

        // parent object placed on `s2d`
        var tile = h2d.Tile.fromColor( 0x0000FF, 120, 120 );
        var b = new h2d.Bitmap( tile, s2d );
        b.setPosition( s2d.width/2, s2d.height/2 );

        parent_obj = b; // save reference

        // 1. child (label for parent)
        var t = new h2d.Text( hxd.res.DefaultFont.get(), b );
        t.text = "parent object";

        // 2. child ("sub-bitmap")
        var tile = h2d.Tile.fromColor( 0x00FF00, 45, 45 );
        var bb = new h2d.Bitmap( tile, b );
        bb.setPosition( 30, 50 ); // will be relative to parent!

        // save second reference
        a_child_obj = bb;

        // 3. child (label for "sub-bitmap")
        var t = new h2d.Text( hxd.res.DefaultFont.get(), bb );
        t.text = "\"sub-bitmap\"";

        // control buttons
        var flow = new h2d.Flow( s2d ); flow.layout = h2d.Flow.FlowLayout.Vertical;
        var add_button = (infotext:String) -> {
            var ia = new h2d.Interactive( 250, 32, flow );
            var t = new h2d.Text( hxd.res.DefaultFont.get(), ia );
            t.text = infotext;
            return ia;
        };
        var bt = add_button( "rotate parent object (click here)" );
        bt.onClick = (e)->{ parent_obj.rotate( 0.2 ); };
        var bt = add_button( "rotate child object (click here)" );
        bt.onClick = (e)->{ a_child_obj.rotate( 0.2 ); };
        var bt = add_button( "random alpha for child object (click here)" );
        bt.onClick = (e)->{ a_child_obj.alpha = 0.1 + hxd.Math.random( 0.9 ); };
    }
}
```

---
## Sample 2

The second sample here is a demonstration of how object trees are useful when finally having many objects "treed" together. Referencing single objects within the tree allows controlling all sub-branches of the tree, also making programming easy and powerful.

### Preview

![objects_sample_when_running](https://user-images.githubusercontent.com/88530062/174419588-1ca660b6-0cb5-4c92-ab15-f715ef88cfc5.png)

### Code

```haxe
class Main extends hxd.App {
    // object references
    var clock : h2d.Graphics;
    var npc : h2d.Graphics;
    // more control variables
    var clock_deltaScale = -0.01;
    var npc_moveX : Float = 0;
    var npc_moveY : Float = 0;
    var npc_deltaAlpha : Float = -0.01;
    var transitionsActive : Bool = false;
    //var npc_trans
    static function main() {new Main();}
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
        var t = new haxe.Timer(1000); // simple Haxe timer for calling a function to move the hands
        t.run = ()->{
            handHours.rotate( Math.PI/3600 );
            handMinutes.rotate(  Math.PI/60 );
        };
        // ball on hand for minutes will move with the hand/indicator
        var g = new h2d.Graphics( handMinutes ); // because it's its child
        g.setPosition( 60, 0 ); g.beginFill( 0x0 ); g.drawCircle( 0, 0, 8 );

        npc = new h2d.Graphics( s2d );
        npc.setPosition( 300, 300 );

        // NPC's face
        npc.beginFill( 0xFFFF00 ); npc.drawCircle( 0, 0, 40 );
        // first eye
        npc.beginFill( 0x0 ); npc.drawCircle( -20, 0, 7 );
        // second eye
        npc.beginFill( 0x0 ); npc.drawCircle( 20, 0, 7 );
        // a happy mouth
        npc.beginFill( 0x0 ); npc.drawPie( 0, 10, 15, 0*Math.PI, 1*Math.PI );
        // and some eye sparkle
        npc.beginFill( 0xFFFFFF ); npc.drawCircle( -20, -5, 3 );
        npc.beginFill( 0xFFFFFF ); npc.drawCircle( 20, -5, 3 );

        var healthbar = new h2d.Graphics( npc );
        healthbar.setPosition(-50,-70);
        healthbar.beginFill( 0xFF0000 ); healthbar.drawRect( 0, 0, 100, 20 );
        healthbar.beginFill( 0x00FF00 ); healthbar.drawRect( 0, 0,  70, 20 );

        var healthbar_label = new h2d.Text( hxd.res.DefaultFont.get(), healthbar );
        healthbar_label.text = "NPC\n70 lifepoints left";
        healthbar_label.setPosition( 0, -32 );

        var npc_moveVectorLabel =  new h2d.Text( hxd.res.DefaultFont.get(), npc );
        npc_moveVectorLabel.setPosition(-50,50);
        // defining a timer to change the NPC's movement vector
        var t = new haxe.Timer(3*1000); // change moving direction every 5 seconds
        t.run = ()->{
            npc_moveX = Math.round( ((-1+(Math.random()*2))) * 100) / 100;
            npc_moveY = Math.round( ((-1+(Math.random()*2))) * 100) / 100;
            npc_moveVectorLabel.text = 'move vector (x: ${npc_moveX}, y: ${npc_moveY})';
        };

        // button to control additional transitions (clock size & NPC alpha)
        var button = new h2d.Interactive( 200, 48, s2d ); button.backgroundColor = 0xFF0000FF;
        button.setPosition( 0, s2d.height - 48 );
        var t = new h2d.Text( hxd.res.DefaultFont.get(), button ); t.text = "Click to activate\nall transitions";
        button.onClick = (e) -> {
            transitionsActive = !transitionsActive;
            t.text = 'changes on clock scale\n& NPC\'s alpha: ${ ( transitionsActive ? "active" : "inactive" ) }';
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
        // additional transitions: clock size & NPC alpha
        if( transitionsActive ){
            // npc changes transparency
            if( npc.alpha < 0.1 || npc.alpha > 1 )
                npc_deltaAlpha = -npc_deltaAlpha;
            npc.alpha += npc_deltaAlpha;
            // clock changes size
            if( clock.scaleX < 0.8 || clock.scaleX > 1 )
                clock_deltaScale = -clock_deltaScale;
            clock.scaleX += clock_deltaScale;
            clock.scaleY += clock_deltaScale;
        }
    }
}
```

Have a look at the last block of code after `if( transitionsActive ){ `...
Note that we only changed the `clock`s scale and `npc`'s `alpha` and all their children – though very own objects – changed with them!

Side note: Here by using `haxe.Timer` we rely on simple [Haxe API](https://api.haxe.org/) to provide another way besides the `update` method to make changes
