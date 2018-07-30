# Events and interaction

Making objects interactive is done creating a [`h2d.Interactive`](api/h2d/Interactive.html) instance. You provide it an interaction area and attach it to a sprite.

```haxe
var interaction = new h2d.Interactive(300, 100, mySprite);
interaction.onOver = function(event : hxd.Event) {
	mySprite.alpha = 0.7;
}
interaction.onOut = function(event : hxd.Event) {
	mySprite.alpha = 1;
}
interaction.onPush = function(event : hxd.Event) {
	trace("down!");
}
interaction.onRelease = function(event : hxd.Event) {
	trace("up!");
}
interaction.onClick = function(event : hxd.Event) {
	trace("click!");
}
```

## Global events

You can listen to global events (keyboard, touch, mouse, window) by adding event listener to the [`hxd.Stage`](api/hxd/Stage.html) instance.

```haxe
function onEvent(event : hxd.Event) {
	trace(event.toString());
}
hxd.Stage.getInstance().addEventTarget(onEvent);
```
Don't forget to remove the event using removeEventTarget when disposing your objects.

## Keyboard events

Keyboard events can be observerd using the global event, check if the `event.kind` is `EKeyDown` or `EKeyUp`.

```haxe
function onEvent(event) {
	switch(event.kind) {
		case EKeyDown: trace('DOWN keyCode: ${event.keyCode}, charCode: ${event.charCode}');
		case EKeyUp: trace('UP keyCode: ${event.keyCode}, charCode: ${event.charCode}');
		case _:
	}
}
hxd.Stage.getInstance().addEventTarget(onEvent);
```

You can use the static functions `hxd.Key.isPressed`, `hxd.Key.isDown` and `hxd.Key.isReleased`.

```haxe
if (Key.isPressed(Key.SPACE)) {
	trace("shoot!");
}
```

## Resize events

You can listen to resize events by adding `addResizeEvent` listener to the [`hxd.Stage`](api/hxd/Stage.html) instance.

```haxe
function onResize() {
	var stage = hxd.Stage.getInstance();
	trace("Resized to ${stage.width}px * ${stage.height}px');
}
hxd.Stage.getInstance().addResizeEvent(onResize);
```
Don't forget to remove the event using removeEventTarget when disposing your objects.


All events callbacks in Heaps receive a [`hxd.Event`](api/hxd/Event.html) instance, which contains info about the event.
