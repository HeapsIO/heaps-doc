# Sprites

The following properties and methods can be accessed on any Sprite:

* `x` and `y` : the position in pixels relative to the parent Sprite (or in the Scene)
* `rotation` : the rotation of the sprite in radians
* `scaleX` and `scaleY` : the horizontal and vertical scaling values for this Sprite (default to (1,1)). You can uniformaly increase the current scales by calling sprite.scale(1.1) or set them to give value by using sprite.setScale(value).
* `visible` : when visible is set to false, a sprite is still updated (position is calculated and animation still plays) but the sprite and all its children are not displayed
* `parent` : the current parent Sprite, or null if it has not been added
* `remove()` : remove the sprite from its parent. This will prevent it from being updated and displayed
* `addChild()` : adds the specified sprite to the children list
* `for( s in sprite ) {...}` : iterates over all the current children

Sprite instances have other properties and methods which can be discovered by visiting the [`h2d.Sprite`](https://github.com/ncannasse/heaps/blob/master/h2d/Sprite.hx) API section.
