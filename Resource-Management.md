# Resource management

Heaps provides a complete asset/resource management framework that can be customized with your own asset packaging system.

## Getting Started

You can directly access your resources in a typed manner in Heaps by using the `hxd.Res` class, for instance by doing:

```haxe
var tile = hxd.Res.myDirectory.myBitmap.toTile();
```

Please note that this is strictly typed: hxd.Res will look into the `res` directory of your project (or the directory specified by `-D resourcesPath=...` haxe compilation parameter). It will then list all directories and files, and depending on the file extension, it will provide you access to the following resources:

 * `png,jpg,jpeg,gif`: [hxd.res.Image](https://heaps.io/api/hxd/res/Image.html)
 * `wav,mp3,ogg` : [hxd.res.Sound](https://heaps.io/api/hxd/res/Sound.html)
 * `fbx,hmd`: [hxd.res.Model](https://heaps.io/api/hxd/res/Model.html)
 * `fnt` (+png): [hxd.res.BitmapFont](https://heaps.io/api/hxd/res/BitmapFont.html)
 * `ttf`: [hxd.res.Font](https://heaps.io/api/hxd/res/Font.html)
 * `tmx`: [hxd.res.TiledMap](https://heaps.io/api/hxd/res/TiledMap.html)
 * `atlas`: [hxd.res.Atlas](https://heaps.io/api/hxd/res/Atlas.html)
 * other: [hxd.res.Resource](https://heaps.io/api/hxd/res/Resource.html) (still allows you to ready binary data)


## Resources loader

`hxd.Res` gives you strictly typed shortcuts to access your resources, but it does not take care of resource loading. For this, you need to initialize a resource loader before accessing your first resource, for instance:

```haxe
hxd.Res.initEmbed();
```

This is the same as writing:

```haxe
hxd.Res.loader = new hxd.res.Loader(hxd.fs.EmbedFileSystem.create());
```

A loader will cache the resource instances after they have been fetched from the underlying file system. There are several ways to store your resources.

### Dynamic resource resolution

You can resolve a resource from its path in the resource file system by using the following command:

```haxe
hxd.Res.loader.load("path/to/myResource.png")
```

This will return a [hxd.res.Any](https://heaps.io/api/hxd/res/Any.html) which has various methods to transform it to other resources.

### Resources with duplicate names and `Res` completion

Because `Res` does not list the file extensions, there is a possibility for name conflicts between multiple resources, in which case both resources are listed with their extension by using underscore instead of a dot.
For example, `base.png` and `base.json` would be listed as a `base_png` and `base_json` resources in the completion.

However some resource types have so-called paired extensions, which can be found in the [hxd.res.Config](https://github.com/HeapsIO/heaps/blob/master/hxd/res/Config.hx) under `pairedExtensions` variable. When such a file exists, if a file with the same name exists and is listed as a paired extension - it will be omitted from completion. A good example of that is `font.fnt` and `font.png`, which would only be listed as the `font` in `Res` and point at `font.fnt`.

### Runtime resource loading

You can also perform your own runtime loading of resources, by using for example `hxd.net.BinaryLoader`.
Once you have the bytes for your resource, you can use `hxd.res.Any.fromBytes` to transform it into a proper resource.

## File Systems

Resource files are accessed through a virtual file system which implements the [FileSystem](https://heaps.io/api/hxd/fs/FileSystem.html) interface.

Heaps already provides several file systems, such as:

 * [EmbedFileSystem](https://heaps.io/api/hxd/fs/EmbedFileSystem.html) gives access to the resources which are embedded with your code (using haxe `-resource` compilation flag). On platforms such as JavaScript, this allows you to have both your code and assets stored in a single file.
 * [LocalFileSystem](https://heaps.io/api/hxd/fs/LocalFileSystem.html) gives access to a local file system directory where your resources are stored. This requires hard drive access so it is not available in the browser for example.
 * [hxd.fmt.pak.FileSystem](https://heaps.io/api/hxd/fmt/pak/FileSystem.html) reads a `.pak` file which contains all resources packaged into a single binary. The PAK file can be loaded at runtime and several PAK files can be used, the latest loaded being able to override the resources declared in previously loaded PAK files.

You can initialize the resource loader and file system by yourself, or use one of the following shortcuts:

 * `hxd.Res.initEmbed()` for EmbedFileSystem — this will also trigger the embedding of all files present in your resource directory into your code
 * `hxd.Res.initLocal()` for LocalFileSystem
 * `hxd.Res.initPak()` for PAK FileSystem — this will load res.pak, res1.pak, res2.pak, etc. from the local filesystem — until no file is found.

## Building PAK

You can build a `pak` file for all your resources by running the following command from your project directory:

```
haxe -hl hxd.fmt.pak.Build.hl -lib heaps -main hxd.fmt.pak.Build
hl hxd.fmt.pak.Build.hl
```

Options can be found in sources [here](https://github.com/HeapsIO/heaps/blob/master/hxd/fmt/pak/Build.hx#L182)

## PAK Loader

If you want to display a progress bar while loading your PAK file(s), you can override the `loadAssets` method of your `hxd.App` class with the following code:

```haxe
override function loadAssets(done) {
    new hxd.fmt.pak.Loader(s2d, done);
}
```

This will be called before `init()`, and while loading `update()` and `onResize` will not be called.

## Live update

Heaps supports a live update system which can be enabled by compiling application with `-debug` flag or manually setting `hxd.res.Resource.LIVE_UPDATE` to `true`. Additionally, this is supported only when using `LocalFileSystem` (see above).
Currently there are only two resource types that have built-in live update: `Image` and `Sound`.

It is possible to attach custom callbacks for resource updates, which can be done via `hxd.res.Resource.watch` method, see sample code snippet:

```haxe
function reloadParameters() {
  Game.monsterStats = Json.parse(hxd.Res.monster_stats.entry.getText());
  Game.refreshMonsterStats();
}
reloadParameters();
// `reloadParameters` will be called whenever `monster_stats` resource changes.
hxd.Res.monster_stats.watch(reloadParameters);

// When app no longer need to watch the resource, callback can be removed by passing `null` callback.
hxd.Res.monster_stats.watch(null);
```

Things to be aware of:

* Only one callback can be present for a resource. Adding a new one will override the old one.
* Using `watch` on resources with built-in support will break the built-in live update of that resource.

## Target-specific quirks

Some targets do not support specific file formats and file systems, but in some cases that can alleviated.

* On JS target, `ogg` is not supported. It can be enabled by using the [stb_ogg_sound](https://lib.haxe.org/p/stb_ogg_sound/) library. Keep in mind that ogg decoder will not use browser capabilities.
* On HL target, `mp3` is not added to `hxd.Res` listings because it is not a recommended format due to its design (not very suitable for music loops), but it can be loaded manually as well as force-enabled in `Res` by adding the `heaps_enable_hl_mp3` compilation flag.
