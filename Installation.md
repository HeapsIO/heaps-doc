# Installation

In order to get started with Heaps you'll need to do the following:

## Heaps for JavaScript

**1. Download and setup Haxe:**

<a href="https://haxe.org/download"><img src="https://cloud.githubusercontent.com/assets/576184/3142589/5e2c41a0-e9c9-11e3-9608-75ec07df40e7.png" align="left" height="30"/></a> &nbsp;&nbsp;&nbsp; <a href="https://haxe.org/download/">Haxe 3.4.2+</a>

**2. Download and setup Heaps by running:**

```
haxelib install heaps
```

<small>Thats it! You can use Heaps now!</small>

*Having troubles installing Heaps with Haxe 3.4 or higher?* Install the latest git version of Heaps:  `haxelib git heaps https://github.com/ncannasse/heaps.git`

---

## Heaps and HashLink VM
 
In addition to use Heaps.io with [HashLink](http://hashlink.haxe.org) (also known as HL), you need to install: 

 * [HashLink](https://github.com/HaxeFoundation/hashlink/releases) Virtual Machine
 * [Latest Haxe 4 preview](https://haxe.org/download/list/) 
 * Install Haxelib [hldx](https://lib.haxe.org/p/hldx) for DirectX support
 * Install Haxelib [hlopenal](https://lib.haxe.org/p/hlopenal) for OpenAL support
 * Install Haxelib [hlsdl](https://lib.haxe.org/p/hlsdl) for SDL/GL support

---

Once you've downloaded the HashLink binary you'll want to add it to your system PATH.

## Heaps and HashLink/C

 * [SDL](https://www.libsdl.org/download-2.0.php) (place dll in HL directory) Low level access to input and graphics via OpenGL and Direct3D. 
 * [OpenAL 32](https://www.openal.org/downloads/) Cross Platform Audio
 * [VisualStudio 2015 x86 Redistributable](https://www.microsoft.com/en-us/download/details.aspx?id=48145) if you are on Windows.