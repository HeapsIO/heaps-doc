# Sound

Heaps provides sound management. Heaps supports 3 different formats (WAV, MP3, OGG).  The availability of the formats depend on the platform. You can determine which formats are supported using the following example:

```haxe
if(hxd.res.Sound.supportedFormat(Mp3)){
    //Mp3 is available
} 
if(hxd.res.Sound.supportedFormat(OggVorbis)){
    //Ogg format is available
}
```

Sounds can be included in your project by specificying them at compile time in your HXML file

```hxml
-D resourcesPath="../relative/path/to/sound/folder/"
```

Once included in your project you can play a sound file via the file name of the sound you want to access.

```haxe
//If your audio file is named 'my_music.mp3'

var musicResource:Sound = null;
//If we support mp3 we have our sound
if(hxd.res.Sound.supportedFormat(Mp3)){
    musicResource = hxd.Res.my_music;
}  

if(musicResource != null){
    //Play the music and loop it
    musicResource.play(true);
}
```