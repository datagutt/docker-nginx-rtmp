# docker-nginx-rtmp
A Dockerfile installing NGINX and nginx-rtmp-module from source with
default settings for IRL-streaming. Built on Alpine Linux.

* Nginx 1.23.1 (Mainline version compiled from source)
* [nginx-rtmp-module](https://github.com/sergey-dryabzhinsky/nginx-rtmp-module) (Dev branch of Sergey-fork compiled from source)
* Default IRL-streaming settings (See: [nginx.conf](nginx.conf))

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
rtmp://localhost:1935/publish/$STREAM_NAME?psk=secret
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
* URL: `rtmp://localhost:1935/publish`
* Stream Key: `hello?psk=secret`

### Watch Stream
* FFplay: `ffplay -fflags nobuffer rtmp://localhost:1935/publish/hello`

**This image is experimental!*

## Resources
* https://alpinelinux.org/
* http://nginx.org
* https://github.com/sergey-dryabzhinsky/nginx-rtmp-module
* https://obsproject.com
