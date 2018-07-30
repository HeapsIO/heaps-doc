# Flash Player and Adobe AIR

Heaps is able to target Adobe's Flash Player and therefore their Desktop/Mobile publishing tool AIR.

To compile for Flash you can follow this simple example

## Compile for Flash
Make sure to specify the Flash version to be at least 11.8 (with `-swf-version 11.8`) which is required for Heaps.

### Basic hxml

```hxml
# class paths
-cp src

# entry point
-main Main

# libraries
-lib heaps

# output
-swf bin/game.swf
-swf-version 11.8
```

---

