vod-webrtc
==========

setup
-----
$ npm update			# get nodejs dependencies
$ node server.js		# run the signaling server

* Edit the last line of `static/readVideoFile.js`, replace `localhost` with
  your domain name if not testing locally, also replace `/output` with the
  relative path to the wanted video file & metadata.
* Example video & metadata can be downloaded from:
	* http://fruitiex.org/output.webm
	* http://fruitiex.org/output.json
* Serve the static folder on a HTTP server. Your HTTP server must support the
  HTTP Range header, this is what is used for downloading the video in chunks.

chrome-friendly webm files:
---------------------------
$ ffmpeg -i bbb\_sunflower\_2160p\_60fps\_normal.mp4 -vf scale=1920x1080 -threads 4 \
-c:v libvpx -b:v 12M -keyint\_min 150 -g 150 -c:a libvorbis output.webm

if the video still won't play in chrome, fix keyframes to start of chunks:
--------------------------------------------------------------------------
$ export GOPATH=$HOME/go
$ cd $GOPATH
$ go get github.com/acolwell/mse-tools/...
$ bin/mse\_webm\_remuxer path/to/broken.webm path/to/fixed.webm

generate chunk list from a webm file:
-------------------------------------
$ git clone https://github.com/jspiros/python-ebml.git
$ PYTHONPATH=python-ebml python2 utils/webmSegment.py video.webm > video.webm.json
