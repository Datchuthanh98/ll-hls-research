# Docker LHLS Segmenter

segment an incoming TS stream received via TCP and publish it to an LHLS playlist.

Varnish is used as a cached in front of the nodejs webserver

## Testing

You can pipe mpegts output from ffmpeg to port `1234` in the above example and the resulting stream can be played at http://localhost:5000/stream.m3u8

For example on OS X you can run the following command to have ffmpeg stream video from the FaceTime camera (if one is present)

## cmd
sudo su
apt-get update -qq
apt-get install -y --no-install-recommends ca-certificates varnish
ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
apt-get -y clean
rm -r /var/lib/apt/lists/\*
cd Desktop/
cd llhls/
cp varnish.vcl /etc/varnish/default.vcl
chmod +x entrypoint.sh
cd segmenter && npm i && cd ..
cd webserver && npm i && cd ..
bash entrypoint.sh

## how to run server and ffmpeg
## cmd 1
docker run -p 1234:1234 -p 5000:6081 tuanna184006/llhls1

## cmd 2
Di den thu muc chua video, sau do run:

ffmpeg -re -i thaylong1080p.mp4 -c:v libx264 -preset veryfast -g 30 -keyint_min 30 -crf 25 -f mpegts tcp://localhost:1234

Mo tren trinh duyet:   http://localhost:5000/stream.m3u8

