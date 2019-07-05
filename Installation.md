# Installation

In order to get up and running Heaps you'll need to make a decision about how you would like to run and debug your project.

## Installing Haxe and Heaps

**1. Download and setup Haxe:**

<a href="https://haxe.org/download"><img src="https://cloud.githubusercontent.com/assets/576184/3142589/5e2c41a0-e9c9-11e3-9608-75ec07df40e7.png" align="left" height="30"/></a> &nbsp;&nbsp;&nbsp; <a href="https://haxe.org/download/">Haxe 3.4.2+</a>

If you plan on testing with HashLink, however, you will need to download the [Latest Haxe 4 Release Candidate](https://haxe.org/download/list/) from the website.

**2. Download and setup Heaps by running:**

```
haxelib install heaps
```

## Installing Visual Studio Code + Haxe support

The recommended editor for creating Heaps applications is Visual Studio Code:

**1. Download and setup VSCode:**

<a href="https://code.visualstudio.com/"><img src="https://user-images.githubusercontent.com/1022912/45916285-a0959f00-be63-11e8-8f54-8d93e3e4037a.png" align="left" height="30"/></a> &nbsp;&nbsp;&nbsp; <a href="https://code.visualstudio.com/">Visual Studio Code</a>

**2. Install Haxe Extension Pack:**

Click on the **Extension** panel (fourth icon on the left side bar of VSCode), search `haxe` and install **Haxe Extension Pack** that will contain everything you need for Haxe support.

![](https://user-images.githubusercontent.com/1022912/45916335-6547a000-be64-11e8-8d4e-799dffea475f.png)

If your VSCode is up to date (after v1.31), you're good to go. Otherwise, restart VSCode once installation is complete (you can click the `Reload` label that should be showing)

## Additional steps for Heaps and HashLink VM
 
In addition to use Heaps.io with [HashLink](http://hashlink.haxe.org) (also known as HL), you need to install: 

 * [HashLink](https://github.com/HaxeFoundation/hashlink/releases) Virtual Machine
   * On OSX, you will have to follow the instructions laid out [here](https://github.com/HaxeFoundation/hashlink#building-on-linuxosx) to get set up with Hashlink, as the process is a little different.
 * Instead of Haxe 3.4 you need latest [Haxe 4 Release Candidate](https://haxe.org/download/list/)
 * Some additional libraries should be installed with `haxelib install <lib>` command 
   * Install Haxelib [hldx](https://lib.haxe.org/p/hldx) for DirectX support
   * Install Haxelib [hlopenal](https://lib.haxe.org/p/hlopenal) for OpenAL support
   * Install Haxelib [hlsdl](https://lib.haxe.org/p/hlsdl) for SDL/GL support

Once you've downloaded the HashLink binary you'll want to add it to your system PATH [(tutorial)](https://www.computerhope.com/issues/ch000549.htm). 

Once everything is setup, you should be able to run `hl` command from your terminal:

![image](https://user-images.githubusercontent.com/1022912/45916745-4ef11280-be6b-11e8-8d9a-9405508ff014.png)

