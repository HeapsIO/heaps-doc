# Displaying text

[h2d.Text](https://heaps.io/api/h2d/Text.html) can be used to draw text using precomputed bitmap fonts.

### Use text in Heaps

```haxe
var font : h2d.Font = hxd.res.DefaultFont.get();
var tf = new h2d.Text(font);
tf.text = "Hello World\nHeaps is great!";
tf.textAlign = Center;

// add to any parent, in this case we append to root
s2d.addChild(tf);
``` 

For each font family you need a different font file, but you can tint them with code, using `textColor`. 

### HtmlText usage
Along with regular Text, there is also an [h2d.HtmlText](https://heaps.io/api/h2d/HtmlText.html) class that allows to use simple html markdown for rich text.

#### Supported HTML tags and their attributes

* `<font>`: Alters font parameters for tag contents.
  * `face="myFontFace"`: Sets different font face. See remarks about font loading below.
  * `color="#880088"`: Sets text color. Should be in hexadecimal format.
  * `opacity="0.5"`: Set text opacity. Value should be between 0 and 1.
* `<p>`: Inserts line-breaks before and after paragraph and allows to control text alignment.
  * `align="center"`: Set text align for tag contents. Valid values are: `left`, `right`, `center`, `multiline-center`, `multiline-right`.
* `<b>`: Alias to `<font face="bold">`
* `<i>`: Alias to `<font face="italic">`
* `<br/>`: Creates a line-break.
* `<img>`: Inserts an image.
  * `src="myimage.png"`: Name of the image to load. See remarks about image loading beloew.

#### Font and image loading
HtmlText does not use Resource Manager to load fonts or images. Loading is performed by setting the callback methods:
```haxe
// Global callbacks that work for all HtmlText instances.
HtmlText.defaultLoadImage = function( url : String ) : h2d.Tile { return null; }
HtmlText.defaultLoadFont = function( name : String ) : h2d.Font { return null; }
HtmlText.defaultFormatText = function ( text : String ) : String { return text; }
// Per-instance callbacks that allow to customize each instance
// with individual scope.
htmlText.loadImage = function( url : String ) : h2d.Tile { return null; }
htmlText.loadFont = function( name : String ) : h2d.Font { return null; }
htmlText.formatText = function ( text : String ) : String { return text; }
```
It's up to user how to implement resource loading. Please note, that there is no tile/font caching on HtmlText side.  
Lastly, `formatText` allows to modify and/or validate text contents when it's changed.

## Font creation

If you want to use custom fonts, other than the Default one, you need to create a `.fnt` file and put it into your `res` directory.  
Heaps automatically converts all supported `.fnt` files to its own `.bfnt` format to speed up font loading.  
Alternatively it is possible to use Signed Distance Field to generate scalable fonts. See Hiero section for details.

### Supported formats

* All 3 BMFont formats: XML, Text and Binary.  
  _Note: Multiple images per font are not supported._
* [FontBuilder](https://github.com/andryblack/fontbuilder): Divo (discouraged to use, as it is less advanced than BMFont)

### Convert font to bitmap font using BMfont tool
  
> Download BMFont here: <http://www.angelcode.com/products/bmfont/>

In the BMFont-tool, use these settings:

* _File_ → _Font settings_
 * Choose font
 * Choose size 
 * Render from TrueType outline _(if you want smooth fonts)_
* _File_ → _Export settings_
  * Bit depth: 32
  * Preset: white text with alpha (black or outline w/ alpha will do as well, but black isn't tintable)
  * Font descriptor: Any
  * Texture: Png
* _File_ → _Save bitmap font as.._ 
  * Put in your Heaps project under `./res/fonts/myFontName.fnt` 
* _Note:_ Multi-texture fnt files are not supported, ensure that there is only one font texture per fnt file.

### Convert font to bitmap font using Littera

Due to browsers no longer supporting Flash, Littera can be ran only by older browser versions [here](http://www.kvazars.com/littera/) 
or trough standalone flash player by running [swf file](http://www.kvazars.com/littera/littera.swf) directly.  
Use following settings:

* Settings in the left column
  * Update a font from your computer with _Select Font_ button
  * Choose size 
  * Set the values:
    * Select what glyphs do you want to export.
    * Fill: to get a non-gradient color, remove one of the markers dragging it down from the rectangle.
    * Enable/disable stroke
    * Background color
    * ...
* Export settings in the header
  * Format: Either `XML (.fnt)` or `Text (.fnt)`
  * Default values are fine for Heaps
  * Press _Export_ start the downloading process.
  * Unzip the file and put it's content in your Heaps project under `./res/fonts/` 
* If you renamed the files, don't forget to update reference to font texture in `pages` section of `.fnt` file.

### Using libgdx Hiero

[Hiero](https://libgdx.badlogicgames.com/tools.html) can be utilized to generate fonts with extra filters as well as SDF fonts. 

* Refer to this instruction to generate SDF fonts: [Distance Field Fonts](https://github.com/libgdx/libgdx/wiki/Distance-field-fonts)
* Refer to this page for general instruction on Hiero usage: [Hiero](https://github.com/libgdx/libgdx/wiki/Hiero)

### Using fontgen tool

[Fontgen](https://github.com/Yanrishatum/fontgen/) is a command-line utility to generate fonts including SDF and MSDF formats. It was written specifically for Heaps and ShiroGames.  
Utility is strictly command-line with config files, please refer to readme file.

### Be creative with bitmapfonts

If you add some 'padding' (not 'spacing') in the export options, you can decorate the outputted .png-file in your image-editing software with some colors/effects/gradients. You can bake this inside your bitmap.

Want gradients? Its not simple to add them with image-editing software if cells are uneven. Use the "equalize the cell heights" in the export options. Then all chars have the same height and is a bit easier to add the gradients. Of course, the texture will be a bit bigger in that case, since there is a bit more whitespace.

## Using the Font

Once the font is ready, you can use it for your Text:
* initialize the [Resource manager](https://github.com/HeapsIO/heaps/wiki/Resource-Management)
* have a `.fnt` and the corresponding `.png` file in your `res` folder
  * For Divo, `.png` file should have exactly the same name as `.fnt`
  * For BMFont, Heaps will try to find texture referenced in `.fnt` first, and then, if unsuccessful, fallback to same behavior as Divo.
* load it using `hxd.Res.fonts.myFont.toFont()`
* create an `h2d.Text` using this font

### SDF / MSDF
For SDF fonts you need to load fonts slightly differently.

* Instead of `toFont()` use `toSdfFont(size, channel, alphaCutoff, smoothing)`
  * `size` will represent desired font size.
  * `channel` tells which texture channel SDF shader should take the information from.
  * `alphaCutoff` will set the border of a glyph, with `0.5` being default value.
  * `smoothing` controls the amount of antialiasing applied to glyph borders. 
* Set `Text.smoothing` to `true`, otherwise SDF won't render properly.
