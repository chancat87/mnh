# mnh
[![GitHub release](https://img.shields.io/github/v/tag/hzyitc/mnh?label=release)](https://github.com/hzyitc/mnh/releases)

[README](README.md) | [中文文档](README_zh.md)

## Introduction

`mnh` is a tool that makes exposing a port behind NAT possible.

`mnh` will produce an `ip:port` pair for your NATed server which can be used for public access.

```
--------------------------------
| Help Server (NOT behind NAT) |  <------(query for ip:port pair)-------------
--------------------------------                                             |
        ^                                                                    |
        |                                                                    |
        |       ---------(Use some way to send ip:port pair)-------------    |
        |       |                                                       | or |
        |       |                                                       ↓    |
------------------------                  ~~~~~~~~~~~~                ----------
| Service (behind NAT) |    <--------     { Internet }   <----------  | Guests |
------------------------                  ~~~~~~~~~~~~                ----------
```

## Usage

### Pre-requests

* Your server's network type should be [Full-cone NAT](https://en.wikipedia.org/wiki/Network_address_translation#Methods_of_translation).
  > If you don't known what it means, no worries, just continue.

* If your server is behind a firewall or a household router, you probably need to enable [UPnP](https://en.wikipedia.org/wiki/Universal_Plug_and_Play) or do a [port forwarding](https://en.wikipedia.org/wiki/Port_forwarding) to your server on your router since they may block all incoming traffics.

### Setup up a Help server

Please check [mnh_server](https://github.com/hzyitc/mnh_server).

> Only `mnhv1` protocol is currently supported, more protocols will be added in the future, such as [STUN](https://en.wikipedia.org/wiki/STUN).

### Run mnh

```
Usage:
  mnh --server <server> [flags]

Flags:
  -s, --server string    Help server address (Example "server.com:12345")
  -i, --id string        A unique id to identify your machine

  -m, --mode string      Run mode. Available value: demoWeb proxy (default "demoWeb")
  -p, --port int         The local hole port which incoming traffics access to
  -t, --service string   Target service address. Only need in proxy mode (default "127.0.0.1:80")

  -u, --disable-upnp     Disable UPnP

  -h, --help             help for mnh
```

Example:

```
./mnh --server server.com:12345 --id test
```

Expose a local web server:

```
./mnh --server server.com:12345 --id web --mode proxy --service 127.0.0.1:80
```

`mnh` will attempt to request UPnP port forwarding by default.

You can disable it by adding `--disable-upnp`, make sure you have set up port forwarding correctly.
(See [Pre-requests](#pre-requests))

```
./mnh --server server.com:12345 --id web --mode proxy --service 127.0.0.1:80 --port 8888 --disable-upnp
```
