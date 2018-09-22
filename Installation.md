# Installation

In order to get up and running Heaps you'll need to make a decision about how you would like to run and debug your project.

First things first, you're going to need to download the latest version of Haxe. If you plan on testing with HashLink, however, you will need to download the [Latest Haxe 4 Preview](https://haxe.org/download/list/) from the website.

**1. Download and setup Haxe:**

<a href="https://haxe.org/download"><img src="https://cloud.githubusercontent.com/assets/576184/3142589/5e2c41a0-e9c9-11e3-9608-75ec07df40e7.png" align="left" height="30"/></a> &nbsp;&nbsp;&nbsp; <a href="https://haxe.org/download/">Haxe 3.4.2+</a>

**2. Download and setup Heaps by running:**

```
haxelib install heaps
```

## Additional steps for Heaps and HashLink VM
 
In addition to use Heaps.io with [HashLink](http://hashlink.haxe.org) (also known as HL), you need to install: 

 * [HashLink](https://github.com/HaxeFoundation/hashlink/releases) Virtual Machine
   * On OSX, you will have to follow the instructions laid out [here](https://github.com/HaxeFoundation/hashlink#building-on-linuxosx) to get set up with Hashlink, as the process is a little different.
 * Instead of Haxe 3.4 you need latest [Haxe 4 preview](https://haxe.org/download/list/)
 * Some additional libraries should be installed with `haxelib install <lib>` command 
   * Install Haxelib [hldx](https://lib.haxe.org/p/hldx) for DirectX support
   * Install Haxelib [hlopenal](https://lib.haxe.org/p/hlopenal) for OpenAL support
   * Install Haxelib [hlsdl](https://lib.haxe.org/p/hlsdl) for SDL/GL support

Once you've downloaded the HashLink binary you'll want to add it to your system PATH.
