Heaps can benefit from HaxeUI for Heaps.

https://www.haxeui.org/api/getting-started/backends/composite-backends/haxeui-heaps.html

---

Alternatively you can also run the demo from the link above with the following code:

```haxe
package ;

import haxe.ui.core.Screen;
import haxe.ui.Toolkit;

class Main2 extends hxd.App {

    public static function main() {
        new Main2();
    }

    override function init() {
        Toolkit.init( {
            root:           s2d,
            manualUpdate:   false
        } );
        
        Screen.instance.addComponent( new MainView() );
    }

}
```