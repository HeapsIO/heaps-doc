# Installation

In order to get up and running Heaps you'll need to install Haxe (the language), Visual Studio Code (the editor) and Heaps itself (the engine).

## Installing Haxe

Heaps works on Haxe 3.4.2+, though it is recommanded to install Haxe 4+ to enjoy new language features and support the HashLink compiler target.

<img src="https://cloud.githubusercontent.com/assets/576184/3142589/5e2c41a0-e9c9-11e3-9608-75ec07df40e7.png" height="30"/>&nbsp;&nbsp;&nbsp;<a href="https://haxe.org/download/">Download Haxe 4+</a>

## Installing Heaps using haxelib
_[haxelib](https://lib.haxe.org/) is the library manager utility of haxe. It is used to download and install published versions of popular code libraries._

In your command line interface of choice, run:
```
haxelib install heaps
```
_Note that in some cases you might need to log out and log back into your computer for haxlib to be recognized as a command._

You can also install directly from github to get the latest, bleeding-edge version of Heaps.
```
haxelib git heaps https://github.com/HeapsIO/heaps.git
```


## Installing Visual Studio Code

The recommended editor for creating Heaps applications is Visual Studio Code:

<a href="https://code.visualstudio.com/"><img src="https://user-images.githubusercontent.com/1022912/45916285-a0959f00-be63-11e8-8f54-8d93e3e4037a.png" align="left" height="30"/></a> &nbsp;&nbsp;&nbsp; <a href="https://code.visualstudio.com/">Visual Studio Code</a>

If your VSCode is up to date (after v1.31), you're good to go. Otherwise, restart VSCode once installation is complete (you can click the `Reload` label that should be showing, or hit Ctrl+Shift+P and type "Reload Window", then hit Enter)

## Installing Haxe extensions on vscode
This step will setup Haxe language support for vscode, the easier generation of comments and debuggers for some compilation targets.

Click on the **Extension** panel (fourth icon on the left side bar of VSCode), search `haxe` and install **Haxe Extension Pack** that will contain everything you need for Haxe support.

![](https://user-images.githubusercontent.com/1022912/45916335-6547a000-be64-11e8-8d4e-799dffea475f.png)

## Optional (Install the Hashlink VM)

While Haxe and Heaps can out of the box compile to an expanding list of targets, follow these additional steps to be able to compile to [HashLink](http://hashlink.haxe.org), a Haxe virtual machine axed on high performance and quick compilation.

### Install Hashlink
Download and install the [HashLink](https://github.com/HaxeFoundation/hashlink/releases) Virtual Machine

_On OSX, you will have to follow the instructions laid out [here](https://github.com/HaxeFoundation/hashlink#building-on-linuxosx) to get set up with Hashlink, as the process is a little different._

Once you've downloaded the HashLink binary you'll want to add it to your system PATH [(tutorial)](https://www.computerhope.com/issues/ch000549.htm). 

### Update Haxe
Make sure you are using at least [Haxe 4+](https://haxe.org/download/).  
_To confirm your current version of Haxe, type `haxe --version` in the command line._

## Install additional libraries
Using `haxelib install` in the command line, install the following items:  

  * [`hlopenal`](https://lib.haxe.org/p/hlopenal) for OpenAL support
  * [`hldx`](https://lib.haxe.org/p/hldx) for DirectX support
  * [`hlsdl`](https://lib.haxe.org/p/hlsdl) for SDL/GL support

```
haxelib install hlopenal
haxelib install hlsdl
haxelib install hldx
```

## Verify the installation
Once everything is setup, you should be able to run `hl` command from your terminal:

![image](https://user-images.githubusercontent.com/1022912/45916745-4ef11280-be6b-11e8-8d9a-9405508ff014.png)
