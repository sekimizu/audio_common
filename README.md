
# ***** NOTICE *****
This is a ROS package for audio_common running on ROS services. All coding structures are referenced from [audio_common](http://wiki.ros.org/audio_common).

------

## REQUIREMENTS
- Ubuntu 16.04
- ROS Kinetic Kame
- GStreamer
> apt-get install libgstreamer-plugins-base1.0-dev gstreamer1.0-libav gstreamer1.0-plugins-bad gstreamer1.0-plugins-bad-dbg gstreamer1.0-plugins-ugly gstreamer1.0-plugins-ugly-dbg gstreamer1.0-plugins-good gstreamer1.0-plugins-good-dbg

- Festival
> The festival is located in "Linux_for_Tegra/PegaMisc" folder.
> But the package could be re-built by the following steps:

### 1. Download source code from [festival version 2.4](http://festvox.org/packed/festival/2.4/)
#### 1-1. Need to download `festival-2.4-release.tar.gz`, `festival-2.4-release.tar.gz`, `festlex_OALD.tar.gz`, `festlex_OALD.tar.gz` and `speech_tools-2.4-release.tar.gz`

#### 1-2. --speech_tools--
```
$ tar xvf `speech_tools-2.4-release.tar.gz`
$ cd speech_tools
$ ./configure 
$ make
```

#### 1-3. --festival--
```
$ tar xvf `festival-2.4-release.tar.gz`
$ cd festival
$ ./configure 
$ make
$ sudo make install
```

#### 1-4. --plugins--
```
$ tar xvf `festlex_CMU.tar.gz`
$ tar xvf `festlex_OALD.tar.gz`
$ tar xvf `festlex_OALD.tar.gz`
```

#### 1.5. --language package--
The language pkt. could be downloaded from here [voices package](http://festvox.org/packed/festival/2.4/voices/), user could chose whiich pronounce he liked.
Download target voices pkt. and unzip it into "festival/lib/voices" folder.

#### 1-6. --change default pronounce pkt.---
set in command line
```
$ festival (ENTER)
 festival> voice_default (ENTER)
   voice_cmu_us_slt_arctic_hts 
 festival> (voice_ (TAB)
   voice_cmu_us_ahw_cg  voice_cmu_us_aup_cg  voice_kal_diphone    voice_reset
 festival> (voice_cmu_us_ahw_cg) 
```

set in config file
```
$ vim ~/.festivalrc
```
Add "(set! voice_default voice_cmu_us_aup_cg)" into file.

## DESCRIPTIONS
This package is not under active development, but is accepting pull requests for bug fixes and new features. (Development may be done for serious bug fixes; pending maintainer time).

The `sound_play`, `groovy-devel` and `hydro-devel` branches are from previous ROS releases and are frozen; no new pull requests will be accepted on these branches.

The `indigo-devel` branch is the stable branch; only bug fixes are accepted on this branch. Periodic releases are done from `indigo-devel` into ROS Indigo and ROS Jade, with version numbers in the 0.2.x range.

The `master` branch is currently considered the development branch, and is released into ROS Kinetic with version numbers in the 0.3.x range. `master` is accepting new, non-breaking features and bug fixes.

Large, breaking changes such as changes to dependencies or the package API will be considered, but they will probably be staged into a development branch for release into the next major release of ROS (ROS L)

## TEST

```
$ rostopic echo /robotsound
sound: -3
command: 1
volume: 1.0
arg: "Hello world"
arg2: "voice_kal_diphone"
```

```
$ rostopic type /robotsound
sound_play/SoundRequest
```

```
$ rosmsg show sound_play/SoundRequest
int8 BACKINGUP=1
int8 NEEDS_UNPLUGGING=2
int8 NEEDS_PLUGGING=3
int8 NEEDS_UNPLUGGING_BADLY=4
int8 NEEDS_PLUGGING_BADLY=5
int8 ALL=-1
int8 PLAY_FILE=-2
int8 SAY=-3
int8 PLAY_STOP=0
int8 PLAY_ONCE=1
int8 PLAY_START=2
int8 sound
int8 command
float32 volume
string arg
string arg2
```

## USAGE

> TTS

```
$ rostopic pub -1 /robotsound sound_play/SoundRequest "sound: -3
command: 1
volume: 1.0
arg: 'Hello world'
arg2: 'voice_kal_diphone'" 
```

```
$ rostopic pub -1 /robotsound sound_play/SoundRequest "sound: -3
command: 1
volume: 1.0
arg: 'You cannot set the voice with festival.scm; to set voices globally, set order of searched voices in the box'
arg2: 'voice_cmu_us_ahw_cg'" 
```

> FILE PLAYBACK 

```
$ rostopic pub -1 /robotsound sound_play/SoundRequest "sound: -2
command: 1
volume: 1.0
arg: '/shared/audio_files/2.mp3'
arg2: ''"
```

