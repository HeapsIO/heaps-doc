# Drawable

H2D classes that can display something on screen usually extend the [`h2d.Drawable`](https://github.com/ncannasse/heaps/blob/master/h2d/Drawable.hx) class.

Each Drawable (including [`h2d.Bitmap`](https://github.com/ncannasse/heaps/blob/master/h2d/Bitmap.hx) has then several properties that can be manipulated:

* `alpha` : this will change the amount of transparency your drawable is displayed with. For instance a value of 0.5 will display a Tile with 50% opacity.
* `color` : color is the color multiplier of the drawable. You can access its individual channels with (r,g,b,a) components. It is initialy set to white (all components are set to 1.). Setting for instance the (r,g,b) components to 0.5 will make the tile appear more dark.
* `blendMode` : the blend mode tells how the drawable gets composited with the background color when its drawn on the screen. The background color refers to the screen content at the time the sprite is being drawn. This can be another sprite content if it was drawn before at the same position. The following blend mode values are available :  
	
    * `Alpha` (default) : the drawable blends itself with the background using its alpha value. An opaque pixel will erase the background, a fully transparent one will be ignored.
    * `None` : this disable the blending with the background. Alpha channel is ignored and the color is written as-it. This offers the best display performances for large backgrounds that have nothing showing being them
    * `Add` : the drawable color will be added to the background, useful for creating explosions effects or particles for instance.
    * `SoftAdd` : similar to Add but will prevent over saturation
    * `Multiply` : the sprite color is multiplied by the background color
    * `Erase` : the sprite color is substracted to the background color
   
* `filter` : when a sprite is scaled (upscaled or downscaled), by default Heaps will use the nearest pixel in the Tile to display it. This will create a nice pixelated effect for some games, but might not looks good on your game. You can try to set the filter value to true, which will enable bilinear filtering on the sprite, making it looks less sharp and more smooth/blurry.
* `shaders` : each `Drawable` can have shaders added to modify their display. Shaders are be introduced [here](documentation/h2d/shaders.html).

`Drawable` instances have other properties which can be discovered by visiting the [`h2d.Drawable`](https://github.com/ncannasse/heaps/blob/master/h2d/Drawable.hx) API section.
