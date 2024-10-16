In order to convert automatically some [Resources](Resource-Management) present in the `res` directory into a more appropriate runtime format, Heaps file system supports resource baking.

This can be controlled by adding `props.json` configuration files in your `res` directory.

Here's for example a way to convert automatically all the `wav` sound files to `ogg` format:

```json
{
    "fs.convert" : {
        "wav" : "ogg"
    }
}
```

This will look for a `hxd.fs.Convert` registered instance that is capable of transforming `wav` to `ogg` and execute it when before wav file data is accessed. It will do the appropriate transformation and cache the result in `res/.tmp` directory. These tmp files should not be archived as they can easily be rebuilt.

So when reading the `wav` content, you will instead get the `ogg` data. This is not a problem since heaps resources automatically detect the file type based on its data and not from its file extension.

Any resource file system will perform theses conversions:
- LocalFileSystem at runtime, upon accessing the desired file
- EmbedFileSytem at compile time, before embedding all files
- PakFileSystem at PAK building time.

Read [[Resource Management]] for an introduction on file systems.

## Default Configuration

The default convert configuration is set as the following:

```json
{
    "fs.convert" : {
        "fbx" : "hmd",
        "fnt" : "bfnt",
    }
}
```

This will convert all FBX files to the native HMD (Heaps Model) binary file, and the same for the FNT (fonts data).

## Overridding

The default convert as well as each `res` directory convert can be overridden by adding a `props.json` file.

For instance by adding this `props.json` file in `res/characters`, these rules will apply to all files within this directory, but not to other directories:

```json
{
  "fs.convert" : {
        "png" : { "convert" : "dds", "format" : "BC1" },
        "^.*_specular\\.png" : "none",
        "myspecifictexture.png" : { "convert" : "dds", "format" : "BC3" }
  }
}
```

Each convert line is a _Rule_. Rules have the following possible syntax, by order of ascending default priority:

- if `[a-z]+`: match a file with the given extension (case insensitive)
- if starts with `^`: a regular expression that will be checked again both the file name and the full file path (will match if one of the two match
- if none of the above: a specific file name (will apply to all files within the directory and sub directories having this exact name)

On the right side of each _Rule_, there's a convert _Command_. Commands have the following possible syntaxes:

- `"none"`: disable conversion, keep original file data
- `"xxx"` : any string identifying a convert. Usually a file extension to convert to.
- `{ "convert" : "name", ...args }`: a convert with some specific arguments.
- `{ "convert" : "name", "priority" : 5 }`: adjust manually the priority in terms of conflicting rules.
- `{ "convert" : "name", "then" : <Command> }`: perform another convert command one this convert as run, enabling consecutive resources baking.

## Custom Converts

Although Heaps already provide (some converts)[https://github.com/HeapsIO/heaps/blob/master/hxd/fs/Convert.hx] you can of course write your own in order to perform any resource transform your project needs.

Here's an example:

```haxe
class MyCustomConvert extends hxd.fs.Convert {
    function new() {
        super("foo","bar"); // converts .foo files to .bar
    }
    override function convert() {
        // make a simple copy
        var bytes = sys.io.File.getBytes(srcPath);
        sys.io.File.saveBytes(dstPath, bytes);
    }
    // register the convert so it can be found
    static var _ = hxd.fs.Convert.register(new MyCustomConvert());
}
```

Then add the following to your `res/props.json` file:

```json
{
    "fs.convert" : {
        "foo" : "bar"
    }
}
```

Depending on your file system, there are different ways to make sure the convert will be found:
- if using the LocalFileSystem, simply compile it as part of your project
- if using the PakFileSystem, compile it together with the Pak Builder script before running it
- if using the EmbedFileSystem, you need to compile it with macros, for instance by adding `--macro MyCustomConvert.doNothing()` to your Haxe project command line parameters and adding a `public static function doNothing() {}` on `MyCustomConvert`

## Multiple configurations

In order to setup multiple configurations, you can have different sections in your `props.json`:

```json
{
    "fs.convert" : {
        "wav" : "ogg"
    },
    "fs.convert.android" : {
        "jpg" : { "convert" : "resize", "scale" : 0.5, "then" : "etc1" }
    },
    "fs.convert.ios" : {
        "jpg" : "astc"
    }

}
```

In order to switch configuration, you can use the following:
- for LocalFileSystem, use `hxd.Res.initLocal(configurationName)`
- for EmbedFileSystem, use `hxd.Res.initEmbed(configurationName)`
- for Pak File System, run pak builder with `-config <configurationName>`, this will output a `res.<config>.pak` file that you can load using `hxd.Res.initPak("res.<config>")`

