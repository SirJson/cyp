# CYP: Control Your Player

## What's the purpose of this fork?

This is my personal fork where you find a bunch of WIP stuff and experiements that improvement the CYP user experience. When I started this fork I found this player to be a lightweight, good looking solution for people who want to control a MPD system without running a Linux distro or want to use mobile apps all the time. Most fixes and improvments will for now be focused on allowing CYP to be a more complete experience especially on desktops and with non-developers.

What I might be working on can be found in the projects section.

I will still see if I can upstream what is needed or wanted upstream because I do not plan to fork and rebrand this as my project and I think I might not be the only one benefiting from these improvments.

I also understand that not all of my issues align with what the original author intended, meaning I can't say what will make it and if something will make it at all. 

**Don't take my word for anything until there is a upstream PR with a changeset.**

---

CYP is a web-based frontend for [MPD](https://www.musicpd.org/), the Music Player Daemon. You can use it to control the playback without having to install native application(s). It works in modern web browsers, both desktop and mobile.

## Screenshots

![](misc/screen1.png) ![](misc/screen2.png)


## Features
  - Control the playback, queue, volume
  - Save and load playlists
  - Browse the library by artists/albums/directories
  - Display album art via native MPD calls (no need to access the library; requires MPD >= 0.21)
  - [Youtube-dl](https://ytdl-org.github.io/youtube-dl/index.html) integration
  - Dark/Light themes


## Installation

Make sure you have a working MPD setup first and Node version >= 10

```sh
git clone https://github.com/ondras/cyp.git && cd cyp
npm i
node .
```

Point your browser to http://localhost:8080 to open the interface. Specify a custom MPD address via a `server` querystring argument (`?server=localhost:6655`).

## Instalation - Docker

Alternatively, you can use Docker to run CYP.

```sh
git clone https://github.com/ondras/cyp.git && cd cyp
docker build -t cyp .
docker run --network=host cyp
```

## Youtube-dl integration

You will need a working [youtube-dl](https://ytdl-org.github.io/youtube-dl/index.html) installation. Audio files are downloaded into the `_youtube` directory, so make sure it is available to your MPD library (use a symlink).

If you use Docker, you need to mount the `_youtube` directory into the image:

```sh
docker run --network=host -v "$(pwd)"/_youtube:/cyp/_youtube cyp
```


## Changing the port

...is done via the `PORT` environment variable. If you use Docker, the `-e` switch does the trick:

```sh
docker run --network=host -e PORT=12345 cyp
```

## Password-protected MPD

Create a `passwords.json` file in CYPs home directory. Specify passwords for available MPD servers:

```json
{
  "localhost:6600": "my-pass-1",
  "some.other.server.or.ip:12345": "my-pass-2
}
```

Make sure that hostnames and ports match those specified via the `server` querystring argument (defaults to `localhost:6600`).

## Technology
  - Connected to MPD via WebSockets (using the [ws2mpd](https://github.com/ondras/ws2mpd/) bridge)
  - Token-based access to the WebSocket endpoint (better than an `Origin` check)
  - Written using [*Custom Elements*](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements)
  - Responsive layout via Flexbox
  - CSS Custom Properties
  - SVG icons (Material Design)
  - Can spawn Youtube-dl to search/download audio files
  - Album art retrieved directly from MPD (and cached via localStorage)
