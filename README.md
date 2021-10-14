# Docker LHLS Segmenter

segment an incoming TS stream received via TCP and publish it to an LHLS playlist.

Varnish is used as a cached in front of the nodejs webserver

## Testing

You can pipe mpegts output from ffmpeg to port `1234` in the above example and the resulting stream can be played at http://localhost:8080/stream.m3u8

For example on OS X you can run the following command to have ffmpeg stream video from the FaceTime camera (if one is present)

## cmd

apt-get update -qq
apt-get install -y --no-install-recommends ca-certificates varnish
ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
apt-get -y clean
rm -r /var/lib/apt/lists/\*
cd Desktop/
cd llhls/
cp varnish.vcl /etc/varnish/default.vcl
chmod +x entrypoint.sh
cd segmenter/
npm i
cd ../webserver/
npm i
cd ..
bash entrypoint.sh

ffmpeg -re -i beautiful_in_white_1080p.mp4 -c:v libx264 -preset veryfast -g 30 -keyint_min 30 -crf 25 -f mpegts tcp://localhost:1234
