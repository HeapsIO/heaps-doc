# Debugging

Heaps supports debugging for JavaScript, Flash and HashLink targets.  Debugging is support via [Visual Studio Code](https://code.visualstudio.com/), which is the preferred IDE for Heaps development.

# JavaScript

In order to debug JavaScript you need to install the [Chrome Extension for Visual Studio Code](https://github.com/Microsoft/vscode-chrome-debug).  You'll also need to create source maps so that the Chrome debugger will know how to map your generated JavaScript to the correct location in your Haxe code.

Compiling with the -debug flag will create a source map (.map) alongside the .js file. Source maps can also be generated for release builds with -D source-map. For more information on Haxe source maps see [this artcile.](https://haxe.org/manual/debugging-source-map.html)

```
#Specifying debug will generate source maps
-debug 

#Source maps can also be generated explicitly.
-D source-map
```

Once your Chrome Debugger extension is installed you need to enable it in the debug panel of Visual Studio Code.  From the drop down menu choose 'Add Configuration' then select 'Chrome' from the following menu, then 'Chrome Launch'.  This will generate a launch.json file.  

![Chrome Debugger Panel](img/debugging/debugconfig.jpg)

Your 'launch.json' file will look something like the following.  Note that the port should match the port of your server environment.  Visual Studio Code includes several live server extensions for you run as a local server.

![Chrome Debugger launch.json](img/debugging/launch.jpg)

The Visual Studio Code debugging panel dropdown menu will now include an option to 'Launch Chrome'.  Select that and a new window should get created that the debugger can attach to.  If you have generated source maps for your build and set a break point the debugger should stop on the break and allow you to inspect.

![Visual Studio Code debugging Heaps](img/debugging/break.jpg)

# HashLink

HashLink supports debugging in Visual Studio Code*.  To get strated with debugging  HashLink you need to install the 'HashLink Debugger Extension'.  Although Heaps supports Haxe 3.4.7, HashLink has Haxe 4.0 as a requirement for debugging.

*Currently only Windows and Linux support debugging, OSX will be added at a later date.

![HashLink Debugger Extension](img/debugging/hldebuggerext.jpg)

To configure the HashLink debugger go to the debug panel of Visual Studio Code. From the drop down menu choose 'Add Configuration' then select 'HashLink' from the following menu. This will generate a launch.json file. You should be able to then select 'HashLink' from the debug configuration menu.

![HashLink Debugger Extension](img/debugging/debugconfig.jpg)

If you set a breakpoint in your code when it launches you should see something like the following.

![HashLink Debugger in Visual Studio Code](img/debugging/hldebug.jpg)

# Flash

You can debug your SWF files as well via the Flash Debugger extension for Visual Studio Code. 

![Flash Debugger Extension](img/debugging/fdb.jpg)

Your SWF needs to be compiled correctly for the debugger to work.  Make sure that '-d fdb' is added to your HXML file.  Please refer to the [Flash Debugger page](https://github.com/vshaxe/flash-debugger) on how to configure it correctly. 

```
#Your SWF compile needs to include the following for the debugger to work
-D fdb

# output
-swf bin/game.swf
-swf-version 11.8
```

To configure the Flash Debugger go to the debug panel of Visual Studio Code. From the drop down menu choose 'Add Configuration' then select 'Flash' from the following menu. This will generate a launch.json file. You should be able to then select 'Flash' from the debug configuration menu.  You should then see 'Flash' as an option from the debugger menu.

If you set a breakpoint in your code when it launches you should see something like the following.

![Flash Debugger in Visual Studio Code](img/debugging/flashdebug.jpg)




