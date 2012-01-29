
Node libavcodec bindings
=

*navcodec* is a module that aims to provide a node wrapper for the excellent [libavcodec](http://libav.org) library. The module does not aim to make a 1 to 1 mapping between this library and javascript. Instead the aim is to create a new javascript API inspired by the underlying library that is more javascript oriented and easier to use.

The initial priority will be to make the transcoding use case, and later move to other uses cases such as playing, filtering, editing, etc.

Transcoding
-

	navcodec = require('navcodec');
	
	navcodec.open('myinput.mov', function(err, media){
	  if(media){
	    media.addOutput('myoutput.mp4', {
	    	width:640,
	    	height:480,
	    	audioBitrate:128000,
	    	videoBitrate:500000
	    });
	    
	    media.transcode(function(err, progress, time){
	      console.log(progress);
	      if(progress === 100){
	        console.log('total transcoding time:'+time);
	      }
	    }
	  }
	});


This will transcode myinput.mov into myoutput.mp4 according to the given options. The callback will be called with a progress variable between 0 and 100.

Since the transcode uses mostly *libavcodec* optimized functions, the above example will run really fast.

Available options (and their defaults):

    width ( input width )
    height (input height )
    videoBitrate (input bitrate if available, 0 otherwise)
    videoCodec ( standard codec for current file container)
    keepAspectRatio (true)
    audioBitrate (input bitrate if available, 0 otherwise)
    sampleRate (44100)
    channels (2),
    audioCodec (standard codec for current file container)
    skipAudio (false)
    skipVideo (false)
    maxVideoFrames (inf)
    videoFrameInterval (1)
    


Thumbnails
-

Several outputs can be added to the media object, and when transcoding the data will be processed in parallel. This is quite convenient when generating thumbnails (which will be very cheap to generate), or if several output formats are required (will only require one decoding process of the input file):

navcodec.open('myinput.mov', function(err, media){
  if(media){
    media.addOutput('myoutput.mp4', {
    	width:640,
    	height:480,
    	audioBitrate:128000,
    	videoBitrate:500000
    });
    
    media.addOutput('thumbnail.mp4', {
    	width:80,
    	height:60,
    	skipAudio:true,
    	videoBitrate:50000
    });
    
    media.transcode(function(err, progress, time){
      console.log(progress);
      if(progress === 100){
        console.log('total transcoding time:'+time);
      }
    }
  }
});


Metadata
-

Metadata is available after opening the media file. Its just a javascript object with keys and values:

navcodec.open('walk.flac', function(err, media){
  if(media){
    console.log(media.metadata);
  }
});

The previous example would result in the following output:

    {
      ARTIST: 'Foo Fighters',
      TITLE: 'Walk',
      ALBUM: 'Wasting Light',
      DATE: '2011',
      track: '11',
      TOTALTRACKS: '11',
      GENRE: 'Rock',
      album_artist: 'Foo Fighters',
      'ALBUM ARTIST': 'Foo Fighters',
      COMMENT: 'EAC V1.0 beta 1, Secure Mode, Test & Copy, AccurateRip, FLAC 1.2.1b -8' 
    }


Metadata is not normalized in any way, which means it is dependent of the input format as well as the encoding software.


Install
-

navcodec depends on [libavcodec](http://libav.org), and this library
must be installed before you can install this module using npm. 

The library needed is libavcodec version 0.8 and above. Version 0.8 includes some new APIs, that are used by this extension.

For most unixes there are packages available, in ubuntu for example use *libavcodec-dev*. If you want you can also compile the source code with your preferred settings, this can be useful if you want to get the maximum performance from the library.

For Mac OSX use brew and install the ffmpeg package.

	brew install ffmpeg

Note that version 0.8 of libavcodec is brand new and the afore mentioned package managers may not have yet updated it.


When the libavcodec dependencies are fulfilled, just use npm to install the package:

	npm install navcodec


  
Compiling libavcodec
-

- Download the latest stable 0.8 release from [libav.org](http://libav.org/download.html#release_0.8)

- Uncompress the tarball, Ex:

    tar -xvf libav-0.8.tar.gz
    cd libav-0.8
  
- Configure your makefile, example:

		./configure  --prefix=/usr/local --enable-shared --enable-gpl --enable-version3 --enable-nonfree --enable-hardcoded-tables --cc=/usr/bin/clang --enable-libfaac --enable-libmp3lame --enable-libtheora --enable-libvorbis --enable-libvpx --enable-libxvid
  
you can get a list of other configuration options using:

	./configure --help
  
- Compile the libraries:

		make
  
- Install

		make install

References
-

[Ubuntu HOWTO: Install and use the latest FFmpeg and x264](http://ubuntuforums.org/showthread.php?t=786095)

Todo

- Make Asynchronous.
- AC35.1 to stereo transcoding.
- Transcode one input file to multiple outputs.
- Thumbnails generation.
- Metadata support.
- multiple pass encoding.
- Fix Memory Leaks.

License
-

Copyright(c) 2012 Optimal Bits Sweden AB. All rights reserved.
 
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to
deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE SOFTWARE.
 

