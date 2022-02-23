# docker-nginx-rtmp
A Dockerfile installing NGINX, nginx-rtmp-module and FFmpeg from source with
default settings for HLS live streaming. Built on Alpine Linux.

* Nginx 1.21.6 (Mainline version compiled from source)
* nginx-rtmp-module 1.2.2 (compiled from source)
* ffmpeg 5.0 (compiled from source)
* Default HLS settings (See: [nginx.conf](nginx.conf))

[![Docker Stars](https://img.shields.io/docker/stars/datagutt/nginx-rtmp.svg)](https://hub.docker.com/r/datagutt/nginx-rtmp/)
[![Docker Pulls](https://img.shields.io/docker/pulls/datagutt/nginx-rtmp.svg)](https://hub.docker.com/r/datagutt/nginx-rtmp/)
[![Docker Automated build](https://img.shields.io/docker/automated/datagutt/nginx-rtmp.svg)](https://hub.docker.com/r/datagutt/nginx-rtmp/builds/)
[![Build Status](https://travis-ci.org/datagutt/docker-nginx-rtmp.svg?branch=master)](https://travis-ci.org/datagutt/docker-nginx-rtmp)

## Usage

### Server
* Pull docker image and run:
```
docker pull datagutt/nginx-rtmp
docker run -it -p 1935:1935 -p 8080:80 --rm datagutt/nginx-rtmp
```
or 

* Build and run container from source:
```
docker build -t nginx-rtmp .
docker run -it -p 1935:1935 -p 8080:80 --rm nginx-rtmp
```

* Stream live content to:
```
rtmp://localhost:1935/stream/$STREAM_NAME
```

### SSL 
To enable SSL, see [nginx.conf](nginx.conf) and uncomment the lines:
```
listen 443 ssl;
ssl_certificate     /opt/certs/example.com.crt;
ssl_certificate_key /opt/certs/example.com.key;
```

This will enable HTTPS using a self-signed certificate supplied in [/certs](/certs). If you wish to use HTTPS, it is **highly recommended** to obtain your own certificates and update the `ssl_certificate` and `ssl_certificate_key` paths.

I recommend using [Certbot](https://certbot.eff.org/docs/install.html) from [Let's Encrypt](https://letsencrypt.org).

### Environment Variables
This Docker image uses `envsubst` for environment variable substitution. You can define additional environment variables in `nginx.conf` as `${var}` and pass them in your `docker-compose` file or `docker` command.


### Custom `nginx.conf`
If you wish to use your own `nginx.conf`, mount it as a volume in your `docker-compose` or `docker` command as `nginx.conf.template`:
```yaml
volumes:
  - ./nginx.conf:/etc/nginx/nginx.conf.template
```

### OBS Configuration
* Stream Type: `Custom Streaming Server`
* URL: `rtmp://localhost:1935/stream`
* Stream Key: `hello`

### Watch Stream
* FFplay: `ffplay -fflags nobuffer rtmp://localhost:1935/stream/hello`

### FFmpeg Build
```
$ ffmpeg -buildconf

ffmpeg version 4.4 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 10.2.1 (Alpine 10.2.1_pre1) 20201203
  configuration: --prefix=/usr/local --enable-version3 --enable-gpl --enable-nonfree --enable-small --enable-libmp3lame --enable-libx264 --enable-libx265 --enable-libvpx --enable-libtheora --enable-libvorbis --enable-libopus --enable-libfdk-aac --enable-libass --enable-libwebp --enable-postproc --enable-avresample --enable-libfreetype --enable-openssl --disable-debug --disable-doc --disable-ffplay --extra-libs='-lpthread -lm'
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libavresample   4.  0.  0 /  4.  0.  0
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100

  configuration:
    --prefix=/usr/local
    --enable-version3
    --enable-gpl
    --enable-nonfree
    --enable-small
    --enable-libmp3lame
    --enable-libx264
    --enable-libx265
    --enable-libvpx
    --enable-libtheora
    --enable-libvorbis
    --enable-libopus
    --enable-libfdk-aac
    --enable-libass
    --enable-libwebp
    --enable-postproc
    --enable-avresample
    --enable-libfreetype
    --enable-openssl
    --disable-debug
    --disable-doc
    --disable-ffplay
    --extra-libs='-lpthread -lm'
```


**This image is experimental!*

## Resources
* https://alpinelinux.org/
* http://nginx.org
* https://github.com/sergey-dryabzhinsky/nginx-rtmp-module
* https://www.ffmpeg.org
* https://obsproject.com
