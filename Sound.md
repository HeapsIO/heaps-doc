# Sound

Heaps provides sound management. Heaps supports 3 different formats (WAV, MP3, OGG).  The availability of the formats depend on the platform (see [Resource manager](https://github.com/HeapsIO/heaps/wiki/Resource-Management#target-specific-quirks) page for details). You can determine which formats are supported using the following example:

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

## Channel, ChannelGroup and SoundGroup

Each Channel instance belong to one SoundGroup and one ChannelGroup that allow to mass-control certain parameters of Channels.  
### Channel
Channel instance represents actively playing sound. It's possible to control following parameters on each Channel:
* `priority`: Priority of the Channel instance (when limiting maximum audible channels, lowest priority Channels get muted when limit is reached)
* `mute`: Mutes channels. Note that playback still continues when channel is muted.
* `volume`: Control volume of the Channel.
* `fadeTo(volume, ?time = 1, ?onEnd: ()->Void)`: Transit channel to specified volume over set period of time with optional callback when transition ends.
* `addEffect(e:Effect)` / `removeEffect(e:Effect)`: Add and remove effects (see below)
* `pause`: Allows to temporarily pause sound playback.
* `duration`: Represents duration of the Sound.
* `position`: Allow to control playback position of the Channel.
* `loop`: Sets channel playback to loop.
* `queue(s:Sound)`: Allows to queue next Sound to play with this Channel instance. It's possible to queue multiple sounds.

### SoundGroup
Sound groups allow to control volume (multiplier), max audioble channels at once and force mono playback on Channel instances.
### ChannelGroup
Channel groups allow to control priority, mute, volume, fading and effects of multiple Channels that bellong to the group. Note that channel priority takes precedence over individual channel priorities.

## Manager
Audio Manager instance provides API to general management of audio. It can be obtained with `hxd.snd.Manager.get()` call.
Primary use is access to masterVolume, spatialization listener, as well as default sound and channel groups.

```haxe
var manager = hxd.snd.Manager.get();
manager.masterVolume = 0.5;
manager.masterChannelGroup.addEffect(new hxd.snd.effect.Pitch(0.5));
manager.masterSoundGroup.maxAudible = 4;
manager.listener.syncCamera();
// Stops all Channels that belong to SoundGroup with a name "myGroupName".
manager.stopByName("myGroupName");
```

## Effects

When playing a sound, it's possible to attach effects to the Channel or ChannelGroup instance. Due to backend difference, some backends may not support all effect types. Following sample showcases usage of pitch effect:

```haxe
var sound = Res.my_sound.play();
// 0.5 pitch would make it sound slowed down.
var pitch = new hxd.snd.Effect(0.5);
sound.addEffect(pitch);
// Returns first effects with specified type
trace(sound.getEffect(hxd.snd.Effect));
// Remove effect from channel
sound.removeEffect(pitch);
```

### Platform-specific limitations

#### HTML5
* Pitch: Due to how WebAudio works, changing pitch gradually can lead to audio cuts. This cannot be avoided due to pitch being applied only once in 128 samples and impossibility to obtain accurate current sample of audio buffer.
* Spatialization: Velocity is not supported.

#### OpenAL (Hashlink)
* Spatialization: Only mono sounds are supported. (Use SoundGroup.mono to force mono playback)