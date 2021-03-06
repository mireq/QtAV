# [QtAV](https://sourceforge.net/projects/qtav)

QtAV is a media playing library based on Qt and FFmpeg. It can help you to write a player
with less effort than ever before.

QtAV has been added to FFmpeg projects page [http://ffmpeg.org/projects.html](http://ffmpeg.org/projects.html)

**QtAV is free software licensed under the term of LGPL v2.1. If you use QtAV or its constituent libraries,
you must adhere to the terms of the license in question.**

#### [Download binaries from sourceforge](https://sourceforge.net/projects/qtav)
#### [Source code on github](https://github.com/wang-bin/QtAV)

### Features

QtAV can meet your most demands

- Hardware decoding suppprt: DXVA2, VAAPI(buggy now), CedarX(e.g. pcDuino)
- Seek, pause/resume
- Video capture
- OSD and custom filters
- Aspect ratio
- Transform video using GraphicsItemRenderer. (rotate, shear, etc)
- Playing frame by frame (currently support forward playing)
- Playing speed control. At any speed.
- Variant streams: locale file, http, rtsp, etc.
- Playing music
- Choose audio channel
- Choose media stream, e.g. play a desired audio track
- Volume control
- Fullscreen, stay on top
- Multiple render engine support. Currently supports QPainter, GDI+, Direct2D, XV and OpenGL(and ES2).
- Dynamically change render engine when playing.
- Multiple video outputs for 1 player
- Region of interest(ROI), i.e. video cropping
- Video eq: brightness, contrast, saturation
- QML support as a plugin. Most playback APIs are compatible with QtMultiMedia module
- Compatiblity: QtAV can be built with both Qt4 and Qt5. QtAV supports
  both FFmpeg(>=0.9) and [Libav](http://libav.org).


### Extensible Framework (work in progress)

  QtAV currently uses FFmpeg to decode video, convert image and audio data, and uses PortAudio to play
  sound. Every part in QtAV is designed to be extensible. For example, you can write your decoder, audio output for particular platform. [Here is a very good example to add cedar hardware accelerated decoder for A13-OLinuXino	](https://github.com/mireq/QtAV/commit/d7b428c1dae66b2a85b7a6bfa7b253980b5b963c)


# For Developers

#### Requirements

1. [FFmpeg](http://ffmpeg.org) (>=0.9)Latest version is recommanded.
 
[![FFmpeg](http://ffmpeg.org/ffmpeg-logo.png)](http://ffmpeg.org)

or [Libav](libav.org) (>=0.8) Latest version is recommanded.

[![Libav](http://libav.org/libav-logo-text.png)](http://libav.org)

2. [Qt 4 or 5](http://qt-project.org/downloads)  
[![Qt](http://qt-project.org/images/qt13a/Qt-logo.png)](http://qt-project.org)
3. [PortAudio v19](http://www.portaudio.com/download.html)  
[![PortAudio Logo](http://www.portaudio.com/images/portaudio_logo.png)](http://www.portaudio.com)[![PortAudio](http://www.portaudio.com/images/portaudio_logotext.png)](http://www.portaudio.com)

or OpenAL ![OpenAL](http://upload.wikimedia.org/wikipedia/zh/2/28/OpenAL_logo.png "OpenAL")

**The required development files for MinGW can be found in sourceforge
page: [depends](https://sourceforge.net/projects/qtav/files/depends)**

#### Build

You can build QtAV with many compilers and on many platforms. You can use gcc, clang, vc to compile it.  
See the wiki [Build QtAV](https://github.com/wang-bin/QtAV/wiki/Build-QtAV) and [QtAV Build Configurations](https://github.com/wang-bin/QtAV/wiki/QtAV-Build-Configurations)

Here is a brief guide:

It's recommend not to build in source dir.  

    cd your_build_dir
    qmake QtAV_project_dir/QtAV.pro
    make

qmake will run check the required libraries at the first time, so you must make sure those libraries can be found by compiler.
Then qmake will create a cache file _.qmake.cache_ in your build dir. Cache file stores the check results, for example, whether portaudio is available. If you want to recheck, run `qmake QtAV_project_dir/QtAV.pro -config recheck`

_WARNING_: If you are in windows mingw with sh.exe environment, you may need run qmake twice.(ISSUE #18)



#### How To Write a Player

Wrtie a media player using QtAV is quite easy.

    WidgetRenderer renderer;
    renderer.show();
    AVPlayer player;
    player.setRenderer(&renderer);
    player.play("test.avi");

For more detail to using QtAV, see the wiki [Use QtAV In Your Project](https://github.com/wang-bin/QtAV/wiki/Use-QtAV-In-Your-Projects) or examples.


QtAV can also be used in **Qml**

    import QtQuick 2.0
    import QtAV 1.3
    Item {
        VideoOut {
            anchors.fill: parent
            source: player
        }
        AVPlayer { //or MediaPlayer
            id: player
            source: "test.mp4"
        }
        MouseArea {
            anchors.fill: parent
            onClicked: player.play()
        }
    }

### How To Contribute

- [Fork](https://github.com/wang-bin/QtAV/fork) QtAV project on github and make a branch. Commit in that branch, and push, then create a pull request to be reviewed and merged.
- [Create an issue](https://github.com/wang-bin/QtAV/issues/new) if you have any problem when using QtAV or you find a bug, etc.
- What you can do: translation, writing document, find or fix bugs, give your idea for this project etc.

#### Contributors

- Wang Bin(Lucas Wang) <wbsecg1@gmail.com>: creator, maintainer
- Stefan Ladage <sladage@gmail.com>: QIODevice support. Wiki about build QtAV for iOS.
- Miroslav Bendik <miroslav.bendik@gmail.com>: Cedarv support. Better qmlvideofx appearance
- Dimitri E. Prado <dprado@e3c.com.br>: issue 70
- theoribeiro <theo@fictix.com.br>: initial QML support
- Vito Covito <vito.covito@selcomsrl.eu>: interrupt callback

For End Users
-------------

#### Player Usage

An simple player can be found in examples. The command line options is

    player [-ao null] [-vo qt|gl|d2d|gdi|xv] [-vd "dxva[;vaapi[;ffmpeg]]"] [url|path|pipe:]

To disable audio output, add `-ao null`

Choose a render engine with _-vo_ option(default is OpenGL). For example, in windows that support Direct2D, you can run

    player -vo d2d filename

To select decoder, use `-vd` option. Value can be _dxva_, _vaapi_ and _ffmpeg_, or a list separated by `;` in priority order. For example:

    player -vd "dxva;ffmpeg" filename

will use dxva if dxva can decode, otherwise ffmpeg will be used.

QMLPlayer has less options now. To use DXVA decoder:

    QMLPlayer-vd "DXVA;FFmpeg" filename


#### Default Shortcuts

- Double click: fullscreen switch
- Ctrl+O: open a file
- Space: pause/continue
- F: fullscreen on/off
- I: switch display quality
- T: stays on top on/off
- N: show next frame. Continue the playing by pressing "Space"
- O: OSD
- P: replay
- Q/ESC: quit
- S: stop
- R: switch aspect ratio
- M: mute on/off
- Up / Down: volume + / -
- Ctrl+Up/Down: speed + / -
- -> / <-: seek forward / backward
- Drag and drop a media file to player
- Wheel: zoom in/out

Before QtAV 1.3.1, the default behavior can be replaced by subclassing QObject and call `void AVPlayer::setPlayerEventFilter(QObject *obj)` (use null to disable).


# TODO

Read https://github.com/wang-bin/QtAV/wiki/TODO for detail.

Screenshots
----------

Use QtAV in QML with OpenGL shaders(example is from qtmultimedia. But qtmultimedia is replaced by QtAV)

![Alt text](https://sourceforge.net/p/qtav/screenshot/QtAV-QML-Shader.jpg "QtAV QML Shaders")

QtAV on Mac OS X

![Alt text](https://sourceforge.net/p/qtav/screenshot/mac.jpg "player on OSX")

IP camera using QtAV. OS: Fedora 18 (some developers from Italy http://www.selcomsrl.eu/)

![Alt text](https://sourceforge.net/projects/qtav/screenshots/ip_camera.jpg "ip camera")

QMLPlayer on ubuntu

![QMLPlayer](https://sourceforge.net/p/qtav/screenshot/QMLPlayer%2BQtAV.jpg "QMLPlayer")

Video Wall

![Alt text](https://sourceforge.net/p/qtav/screenshot/videowall.png "video wall")



***
### [Donate 资助](https://sourceforge.net/p/qtav/wiki/Donate%20%E6%8D%90%E8%B5%A0)

软件由我一人利用空余学习和工作时间开发。如果您觉得不错，可以考虑资助一下

#####What are the financial needs of QtAV?
- Buy hardware for developing and testing purpose. (lack of AMD card now)

Thanks

Now I have received 1050 RMB(about 160$)

[![Alipay](https://img.alipay.com/sys/personalprod/style/mc/top-logo.png)](https://me.alipay.com/lucaswang)

[PayPal ![Paypal](http://www.paypal.com/en_US/i/btn/btn_donate_LG.gif)](https://sourceforge.net/p/qtav/wiki/Donate%20%E6%8D%90%E8%B5%A0)

[![Support via Gittip](https://rawgithub.com/twolfson/gittip-badge/0.1.0/dist/gittip.png)](https://www.gittip.com/wang-bin)


[Gittip ![Gittip](https://www.gittip.com/assets/10.1.51/logo.png)](https://www.gittip.com/wang-bin)

- - -



> Copyright &copy; Wang Bin wbsecg1@gmail.com

> Shanghai University->S3 Graphics, Shanghai, China

> 2013-01-21
