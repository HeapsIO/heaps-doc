# Optimizing bitmaps

Some tips:

 - Use power of 2 sizes for textures
 - For fast tile rendering, use `h2d.TileGroup`
 - Have a lot of separate bitmaps? Combine them in one texture.
 - For particle rendering, use `h2d.SpriteBatch`
 - Use `engine.drawCount` to see draw calls. Less is better.
