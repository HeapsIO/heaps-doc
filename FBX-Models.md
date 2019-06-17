# FBX Models

FBX Models can be exported from various 3D editors and loaded into Heaps.

Heaps uses its own internal 3D format called HMD which is faster to load and process. By default, the Heaps resource manager will automatically convert any FBX it loads into the corresponding HMD and cache the result. See [this section](https://github.com/ncannasse/heaps/wiki/Resource-Management).

# Exporting

In order to export a FBX model that can be load to HMD, please make sure to:

 * export to FBX 2010 (FBX version 7.x), both FBX binary and text format are supported.
 * export using `Z`-up and `-Y`-forward axis.
 * export using BakeAnimation, this will make sure your animation is exactly the same it was created.
 * note for 3ds Max and Maya: Use Game Exporter instead of standard Export.
 * note for Blender 2.8: to avoid armature scaling, use FBX Units Scale in "Apply Scaling" when exporting to FBX. To prevent animation jitters also reduce Simplify to 0 under Animation tab.

# Restrictions

Some restriction apply to the structure of your models when using FBX export:

 * the same Skin cannot be shared among several objects. Merge your objects, using multiple materials if necessary.
 * try to avoid attaching objects to skin bones. Instead attach by code by using the `follow` property.
 * not all animation curves are supported. Please make an issue and provide example model if you encount unsupported animation curves.

# Load and display

In order to add a FBX/HMD model to your scene, you need first to load it with the Resource manager. There are two primary ways of doing so.

## Using `h3d.prim.ModelCache`

Most robust way is to use `ModelCache` class:

```haxe
// Create a model cache
var cache = new h3d.prim.ModelCache();
// Add a model library to cache.
// This is optional, because `loadModel` and `loadAnimation` add it to cache automatically.
// Returns hxd.fmt.hmd.Library
cache.loadLibrary(hxd.Res.myModel);
// Create a model instance. Compared to manual model creation, ModelCache loads textures automatically.
var obj = cache.loadModel(hxd.Res.myModel);
// Load an animation.
var anim = cache.loadAnimation(hxd.Res.myModel);
// play it on the object
obj.playAnimation(anim);

// Clear the cache instance. Note that cache will dispose all cached model textures as well.
cache.dispose();
```

Another thing to note is that how texture loading resolves texture paths. First it tries to load from directly provided `texturePath`, then, if it fails - relative to passed model. E.g. in case of folder structure like that:

```
texture.png
models/myModel.fbx
models/texture.png
```

It will resolve to top-level `texture.png` instead of `models/texture.png` when loading `hxd.Res.models.myModel` if model requests path as `texture.png`.


## Manual loading

Alternatively it is possible to load models directly from Library:

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
