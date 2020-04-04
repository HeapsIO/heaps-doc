# Shiro Games Stack

Our game engine Heaps.io and the underlying technology and toolset are born from twenty years spent creating games, before at [Motion-Twin](https://motion-twin.com) (the makers of Dead Cells) and since 2012 at [Shiro Games](https://shirogames.com) (Evoland, Northgard, Darksburg).

All of these games - 2D and 3D - have been made using a complete stack of libraries and tools that have been open source from the start, and are still evolving and being maintained.

Since I often get asked about how we are making games, I thought it would be nice to share details on all the elements of Shiro's technology stack. It works great for us, so maybe it could be useful for some other companies out there.

![image](https://user-images.githubusercontent.com/1022912/78455515-258b8680-769f-11ea-8a3c-ae4f16fe3a72.png)

## Native Layer

The native layer is mostly written in C with bits of C++. For daily games development we rarely have to deal with it, since we are working instead with a high level languages where hard crashes (mostly) can't happen without having a nice error message.

Performance is critical to us because - while we don't do AAA kind of games - we still want to get great graphics and immersive 60 FPS gameplay, without having to spend our coder's precious time on micro optimizations because of the low level engine limits.

**HashLink VM**

The core component of this strategy is the [HashLink Virtual Machine](https://hashlink.haxe.org), which is a fast strictly typed VM for Haxe programming language. Think about it like JavaVM or Mono (used by Unity), but more oriented towards real time games.
 
The game gets compiled to a cross platform `.hl` file that can be run with HashlinkVM JIT. It can also be compiled to C directly and compiled using any native compiler, which we are using for our console ports on PlayStation, XBox and Nintendo Switch.

Hashlink VM performs very well for classic Object Oriented programming, but also for functional programming style. It's good too at floating point calculus which is important for games, and has been designed so that memory overhead is only minimal for the garbage collector. For instance our 3D game Northgard requires less than 500 MB of memory to run.

Because of the nature of the VM, it means that unless you compile your code to C, your game doesn't have Debug/Release builds like in C++. You can of course have compilation options to have a Development/Release version, but you will always run at full speed, exactly just like the game will be on players computers, and you also get precise call stack traces.

**Native libraries**

While HashLink alone does come with only a small standard library, it can be extended with additional libraries (written in C) to expose new APIs. This requires a bit of knowledge of the VM and to deal with the Garbage Collector, but it's quite a smooth process and if done right allows us to isolate bugs that might come from the low level layer from bugs in application logic.

So far we have native libraries that gets distributed with HashLink such as SDL2, DirectX11, OpenAL(sound), LibUV(sockets), SSL(encryption), and FMT which deals with Zip, Ogg, Png, Jpg and a few other low level file format libraries.

All of these are open source and part of the HashLink repository [here](https://github.com/HaxeFoundation/hashlink)

We also have additional platform API libraries for [Steam](https://github.com/HeapsIO/hlsteam), as well as consoles integration libraries that are only accessible if you have a registered developer.

So main point is that you can easily extend the VM with your own native libraries if you require any, making sure that you are never locked by waiting for support for a particular native feature.

**Native Tools**

Because of the VM nature, it's always necessary to have a good set of low level tools to be able to analyze performances. We have several of these for HashLink VM:

* Hashlink VM has a very efficient native [Debugger](https://github.com/vshaxe/hashlink-debugger) for best day-to-day coding experience, directly integrated into Visual Studio Code.
* for CPU performance, we have recently developed a [Hashlink CPU Profiler](https://github.com/HaxeFoundation/hashlink/wiki/Profiler) that can be used to get insights about where the CPU time is spent in your program. The profiler is non-intrusive so you will be able to profile your game without slowing it down.
* for Memory performance and leaks, you can either measure all allocations performed or dump the current whole process memory for later analysis using the [Memory Profile API](https://api.haxe.org/hl/Profile.html) - _still undocumented, I'll write later about this_
* for graphics performances, we are using the GPU vendor tools such as [NVidia NSight](https://developer.nvidia.com/nsight-visual-studio-edition).

![image](https://user-images.githubusercontent.com/1022912/78455515-258b8680-769f-11ea-8a3c-ae4f16fe3a72.png)

## Programming Language Layer

Now that we have dealt with the native layer (in green), we're moving up the technology stack to the programming language layer.

For all games code and tools development we are using [Haxe](https://haxe.org) programming language. Haxe offers the best mix of strictly typed object oriented, with bits of functional programming, and super-powerful [macro system](https://haxe.org/manual/macro.html) which is used extensively by several of the high level libraries we will present.

Of course as the main designer of Haxe language I'm a bit biased, but it's important to know that every developer at Shiro really enjoy developing with Haxe on a daily basis, and it's a critical tool for our daily productivity. Also, we have recruited developers with various programming backgrounds (C++, C#, Javascript, Python, Java, etc.) and they all have been able to quickly adapt to Haxe and write efficient code.

Haxe is a cross platform programming language, which can output code for many different targets. We are using mostly two targets at Shiro:
- the Hashlink target for our games
- the JavaScript target for our tools _(more on that as part of the tools section below)_

Some popular independent games were also made using Haxe (but not the HashLink or the whole tech stack I'm presenting here), such as: **Papers Please**, **Brawlhalla**, **Dicey Dungeons**, etc.

Haxe is maintained as an independent open source project, thanks to the [Haxe Foundation](https://haxe.org/foundation/), which gets funding by several big companies which are using Haxe for developing various cross platform applications, games but not only.

You can learn more about Haxe on it's [website](https://haxe.org)

![image](https://user-images.githubusercontent.com/1022912/78455515-258b8680-769f-11ea-8a3c-ae4f16fe3a72.png)

## Game Engine : Heaps.io

![image](https://heaps.io/img/h3d/pbr_1.jpg)

[Heaps.io](https://heaps.io) is the game engine that powers our games at Shiro. It covers the following:
- 2D rendering
- 3D rendering
- Sound handling
- Controls (keyboard, mouse, gamepad)
- Resource management 

It's been built to separate the low level platform implementation features from the mid level graphics logics/data. This architecture allows us to integrate new renderers or platforms by just porting a few classes given that the native libraries are been made available in HashLink. Heaps.io supports the following plaforms/renderer:
- HashLink with DirectX11
- HashLink with OpenGL/SDL2
- HashLink/C with NVN (Nintendo Switch SDK native graphics api)
- HashLink/C with GNM (PS4 SDK native graphics api)
- HashLink/C for XBoxOne SDK
- Javascript with WebGL2
*HashLink/C here means that we can only use the Hashlink C output, not the JIT VM

Heaps.io has been designed to be very lightweight and highly customizable. It provides a 2D/3D scene graph and each Object in the scene can be extended to enrich the behavior. The renderer and lighting system can also be entirely replaced, allowing to write a very game-specific rendering pipeline.

Both 2D and 3D are entirely GPU accelerated, and based on GPU shaders. Heaps comes with its own integrated shader language [HxSL](https://heaps.io/documentation/hxsl.html). HxSL is very powerful as instead of writing a single big shader, you will write little individual shader effects that gets assembled and optimized together at runtime.

Heaps.io also abstracts [Resources Management](https://heaps.io/documentation/resource-management.html) and [Baking](https://heaps.io/documentation/resource-baking.html) that allows to deal with all the packaging of raw resources coming from Photoshop, Maya, Blender, etc.

You can learn more about Heaps.io using dedicated website, or browsing its [Documentation](https://heaps.io/documentation/home.html).

![image](https://user-images.githubusercontent.com/1022912/78455515-258b8680-769f-11ea-8a3c-ae4f16fe3a72.png)

# DomKit UI Toolkit

![image](https://user-images.githubusercontent.com/1022912/78458574-af911a80-76b2-11ea-80cf-8196c4a73f53.png)

[DomKit](https://heaps.io/documentation/domkit.html) is our framework for writing UI components. It's our most recent addition to the technology stack so is still evolving.

It allows you to declare your UI in terms of XHTML directly in your code, which allows direct strictly typed data binding with your gameplay logic, then enables styling using CSS just like web development. 

The semantics of the CSS partly follows HTML5 standard but the layout model is specific to Heaps.io, and is more suitable for games UI/UX.

You can also declare your own components and even add extra properties and code that tells how the CSS values should be evaluated to map to your properties data.

Additionally, DomKit library itself is fully standalone so can be used for any UI framework, although Heaps.io benefits from a direct integration.

You can read more about DomKit using the [documentation](https://heaps.io/documentation/domkit.html). 

