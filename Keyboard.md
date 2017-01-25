```haxe
import hxd.Key in K;
```

And inside an `update` function:

```haxe
if (K.isDown(K.Q)) { }
if (K.isDown(K.DOWN)) { }
```

Methods:

```haxe
// The key is currently pressed
K.isDown(k)

// The key has been pressed
K.isPressed(k)

// The key has been released
K.isReleased(k)
```

[More keys](https://github.com/ncannasse/heaps/blob/master/hxd/Key.hx)
