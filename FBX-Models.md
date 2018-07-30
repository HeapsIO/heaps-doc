# FBX Models

FBX Models can be exported from various 3D editors and loaded into Heaps.

Heaps uses its own internal 3D format called HMD which is faster to load and process. By default, the Heaps resource manager will automatically convert any FBX it loads into the corresponding HMD and cache the result. See [this section](https://github.com/ncannasse/heaps/wiki/Resource-Management).

# Exporting

In order to export a FBX model that can be load to HMD, please make sure to:

 * export to FBX 2010 in text format (FBX version 7.x), the FBX binary format is not supported
 * export using Z-up axis
 * export using BakeAnimation, this will make sure your animation is exactly the same it was created

# Restrictions

Some restriction apply to the structure of your models when using FBX export:

 * the same Skin cannot be shared among several objects. Merge your objects, using multiple materials if necessary
 * try to avoid attaching objects to skin bones. Instead attach by code by using the `follow` property.

# Preview

In order to view your HMD and FBX models and their animations, you can use the Heaps FBXViewer which can be found in the `tools/fbx` directory of Heaps. If you want to see what kind of information is stored in the HMD, you can use `Ctrl+S` while in the viewer to save a text dump of the HMD binary.

# Load and display

In order to add a FBX/HMD model to your scene, you need first to load it with the Resource manager this way:

```haxe
// lib is an hxd.fmt.hmd.Library
var lib = hxd.Res.myModel.toHMD();
// create the model instance : the loadTexture is a custom function responsible for loading the model texture
var obj = lib.makeObject(loadTexture);
// add to the scene
s3d.addChild(obj);
// load the animation
var anim = lib.loadAnimation();
// play it on the object
obj.playAnimation(anim);
```

Please note that the HMD library and animations are not cached. If you want to be able to create many instances of the same object in your scene, make sure to cache the values so they can be reused.

A complete example is available in the `samples/skin` directory.
