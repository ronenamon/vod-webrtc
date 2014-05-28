vod-webrtc
==========

chrome-friendly webm files:
---------------------------
$ ffmpeg -i bbb_sunflower_2160p_60fps_normal.mp4 -vf scale=1920x1080 -threads 4 \
-c:v libvpx -b:v 12M -keyint_min 150 -g 150 -c:a libvorbis output.webm

if the video still won't play in chrome, fix keyframes to start of chunks:
--------------------------------------------------------------------------
$ export GOPATH=$HOME/go
$ cd $GOPATH
$ go get github.com/acolwell/mse-tools/...
$ bin/mse_webm_remuxer path/to/broken.webm path/to/fixed.webm

generate chunk list from a webm file:
-------------------------------------
$ git clone https://github.com/jspiros/python-ebml.git
$ PYTHONPATH=python-ebml python2 webmSegment.py video.webm > video.webm.json
