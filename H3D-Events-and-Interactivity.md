# Events and interaction

Making objects interactive is done creating a `h3d.scene.Interactive` instance. You give it a target object and attach it to a parent.

## Interaction in H3D

```haxe
var interaction = new h3d.scene.Interactive(collider, s3d);
interaction.onOver = function(event) {
	trace("over");
}
interaction.onOut = function(event) {
	trace("out");
}
interaction.onClick = function(event) {
	trace("click!");
}
interaction.onPush = function(event) {
	trace("down!");
}
interaction.onRelease = function(event) {
	trace("up!");
}
interaction.onClick = function(event) {
	trace("click!");
}
```

All events callbacks receive a [`hxd.Event`](api/hxd/Event.html) instance, which contains info about the event.
