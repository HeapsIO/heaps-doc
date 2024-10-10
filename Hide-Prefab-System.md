Hide is a middle tool for Heaps.io, available [here](http://github.com/heapsio/hide)

![Hide](https://camo.githubusercontent.com/2cc9b75edfef16fe46b1b241d663d5773806595b6bc40471230166add038655b/68747470733a2f2f686178652e6f72672f696d672f626c6f672f323032302d30342d30362d736869726f67616d65732d737461636b2f686964652e706e67)

Hide supports a prefab system that allows to define custom components and extend the editor.

# Prefab Definition

A Prefab is an object that contains data that can be authored in Hide. Some prefabs also come with either a 2D or 3D display. Prefabs are used for a wide range of things, such as:
- editing 2D and 3D scenes
- shaders setup and preview
- 2D and 3D timeline fxs and particles
- data configuration (castle db, etc.)
- your own custom prefabs

Unlike some other IDEs, prefabs are not meant to be gameplay objects. They are purely data and usually will not contain any gameplay code. They are meant to be setups that are loaded and manipulated by the game to create gameplay. This design allow the **separation of concerns** by splitting the difficult problem of creating a game into two parts:
- prefabs are responsible for editing and storing the game data. The focus is on ease of editing and clean data storage.
- your game code is responsible for consuming the prefabs to run the game

# Prefab life cycle

Here's a prefab example:

```haxe
class MyPrefab extends hrt.prefab.Prefab {

    @:s var myData : Int;

    static var _ = Library.register("myprefab", MyPrefab);

}
```

Prefab data is stored as plain text JSON with newlines, allowing several editors to concurrently edit the data, using merge/diffs conflict resolution and data history to manage the data.

**Registration**

Prefabs have a `type` which they register using `Library.register`. We don't use the class name as it might be renamed or some games can override the default prefab definitions with a custom one.

When loading prefabs, if the data contains prefab types that are not registered, an `hrt.prefab.Unknown` will be created instead, holding the data of the  unregistered prefab.

**Load and Save**

Prefabs are loading their data from JSON by calling the `function load( obj : Dynamic )` function. And they are saving their data to JSON by calling the `function save() : Dynamic` function. In order to automate boilerplate assigns in both load and save, you can use `@:s` metadata to mark a field as serialized, meaning it will be automatically loaded from an saved to JSON.

**Instancing and Clone**

After being loaded from JSON, prefabs are just plain data. In order to create 2D/3D objects in the scene, they need to be instantiated. This is done use `make()` (recursive) or `makeInstance` (for a single prefab). You need to prepare and pass a `Context` object that will contain various references.

Before being loaded however, a prefab will be cloned
(TBC : `@:c`, references)







