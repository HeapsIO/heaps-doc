Heaps provides a complete asset/resource management framework that can be customized with your own asset packaging system.

## Getting Started

You can directly access your resources in a typed manner in Heaps by using the `hxd.Res` class, for instance by doing:

```haxe
var tile = hxd.Res.myDirectory.myBitmap.toTile();
```

Please note that this is strictly typed: hxd.Res will look into the `res` directory of your project (or the directory specified by `-D resourcePath=...` haxe compilation parameter). It will then list all directories and files, and depending on the file extension, it will provide you access to the following resources:

 * `png,jpg,jpeg,gif`: [hxd.res.Image](https://github.com/ncannasse/heaps/blob/master/hxd/res/Image.hx)
 * `wav,mp3,ogg` : [hxd.res.Sound](https://github.com/ncannasse/heaps/blob/master/hxd/res/Sound.hx)
 * `fbx,hmd`: [hxd.res.Model](https://github.com/ncannasse/heaps/blob/master/hxd/res/Model.hx)
 * `fnt` (+png): [hxd.res.BitmapFont](https://github.com/ncannasse/heaps/blob/master/hxd/res/BitmapFont.hx)
 * `ttf`: [hxd.res.Font](https://github.com/ncannasse/heaps/blob/master/hxd/res/Font.hx)
 * `tmx`: [hxd.res.TiledMap](https://github.com/ncannasse/heaps/blob/master/hxd/res/TiledMap.hx)
 * `atlas`: [hxd.res.Atlas](https://github.com/ncannasse/heaps/blob/master/hxd/res/Atlas.hx)
 * other: [hxd.res.Resource](https://github.com/ncannasse/heaps/blob/master/hxd/res/Resource.hx) (still allows you to ready binary data)


## Resources loader

The `hxd.Res` provides your strictly typed shortcuts to access your resources, but it does not take care of the resource loading. For this, you need to initialize a resource loader before accessing your first resource, for instance:

```haxe
hxd.Res.initEmbed();
```

This will be the same as writing:

```haxe
hxd.Res.loader = new hxd.res.Loader(hxd.fs.EmbedFileSystem.create());
```

A loader will cache the resources instances after they have been fetched from the underlying file system. There are several ways to store your resources.

### Dynamic resource resolution

You can resolve a resource from it path in the resource file system by using the following command:

```haxe
hxd.Res.loader.load("path/to/myResource.png")
```

This will return a [hxd.res.Any](https://github.com/ncannasse/heaps/blob/master/hxd/res/Any.hx) which have various methods to transform it to other resources.

### Runtime resource loading

You can also perform you own runtime loading of resources, by using for example `hxd.net.BinaryLoader`.
Once you have the bytes for your resource, you can use `hxd.res.Any.fromBytes` to transform it into a proper resource.

## File Systems

Resources files are accessed through a virtual file system which implements the [FileSystem](https://github.com/ncannasse/heaps/blob/master/hxd/fs/FileSystem.hx) interface. 

Heaps provides already several file systems, such as:
 * [EmbedFileSystem](https://github.com/ncannasse/heaps/blob/master/hxd/fs/EmbedFileSystem.hx) will gives access to the resources which are embedded with your code (using haxe `-resource` compilation flag). On platforms such as JavaScript, this allows you to have both your code and assets stored in a single file.
 * [LocalFileSystem](https://github.com/ncannasse/heaps/blob/master/hxd/fs/LocalFileSystem.hx) which gives access to a local file system directory where your resources are stored. This require hard drive access so it is not available in the browser for example.
 * [hxd.fmt.pak.FileSystem](https://github.com/ncannasse/heaps/blob/master/hxd/fmt/pak/FileSystem.hx) will read a `.pak` file which contains all resources packaged into a single binary. The PAK file can be loaded at runtime and several PAK files can be used, the latest loaded being able to override the resources declared in previously loaded PAK files.  

You can initialize the resource loader and filesystem by yourself, or use one of the following shortcuts:
 * `hxd.Res.initEmbed()` for EmbedFileSystem - this will also trigger the embedding of all files present in your resource directory into your code
 * `hxd.Res.initLocal()` for LocalFileSystem
 * `hxd.Res.initPak()` for PAK FileSystem - this will load res.pak, res1.pak, res2.pak, etc. from the local filesystem - until no file is found.

## Building PAK

You can build a `pak` file for all your ressources by running the following command from your project directory:

```
haxelib run heaps pak
```

## PAK Loader

If you want to load your PAK file(s) with a progress bar showing, you can override the `loadAssets` method of your `hxd.App` class with the following code:

```haxe
override function loadAssets(done) {
    new hxd.fmt.pak.Loader(s2d, done);
}
```

This will be called before `init()`, and while loading `update()` and `onResize` will not be called.


