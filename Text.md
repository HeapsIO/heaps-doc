# Displaying text

`h2d.Text` can be used to draw text using bitmap fonts. It allows you to set textColor, maxWidth, textAlign, letterSpacing and lineSpacing.
It's also possible to get the size of the text with textWidth/textHeight.

### Use text in Heaps

```haxe
var tf = new h2d.Text(hxd.Res.fonts.myFontName.toFont());
tf.text = "Hello World\nHeaps is great!";
tf.textAlign = Center;

// add to any parent, in this case we append to root
s2d.addChild(tf);
``` 

For each font family you need a different font file, but you can tint them with code, using `textColor`. 

## Font creation

### Convert font to bitmap font using BMfont tool

In the BMFont-tool, use these settings:

* _File_ → _Font settings_
 * Choose font
 * Choose size 
 * Render from TrueType outline _(if you want smooth fonts)_
* _File_ → _Export settings_
  * Bit depth: 32
  * Preset: black text with alpha
  * Font descriptor: Text
  * Texture: Png
* _File_ → _Save bitmap font as.._ 
  * Put in your Heaps project under `./res/fonts/myFontName.fnt` 
  
> Download BMFont here: <http://www.angelcode.com/products/bmfont/>

### Convert font to bitmap font using Littera

In [Littera](http://www.kvazars.com/littera/), use these settings:

* Settings in the left column
  * Update a font from your computer with _Select Font_ big button
  * Choose size 
  * Set the values:
    * Select what glyphs do you want to export.
    * Fill: to get a non-gradient color, remove one of the markers dragging it down from the rectangle.
    * Enable/disable stroke
    * Background color
    * ...
* Export settings in the header
  * Default values are fine for Heaps
  * Press _Export_ start the downloading process.
  * Unzip the file an put it's content in your Heaps project under `./res/fonts/` 
  
### Be creative with bitmapfonts

If you add some 'padding' (not 'spacing') in the export options, you can decorate the outputted .png-file in your image-editing software with some colors/effects/gradients. You can bake this inside your bitmap.

Want gradients? Its not simple to add them with image-editing software if cells are uneven. Use the "equalize the cell heights" in the export options. Then all chars have the same height and is a bit easier to add the gradients. Of course, the texture will be a bit bigger in that case, since there is a bit more whitespace.

#### In need of a dummy font? 

You can start with a basic font provided here: <https://github.com/HeapsIO/heaps/tree/master/samples/res>
