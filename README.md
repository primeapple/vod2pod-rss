# vod2pod-rss [![tests](https://github.com/madiele/vod2pod-rss/actions/workflows/rust.yml/badge.svg)](https://github.com/madiele/vod2pod-rss/actions/workflows/rust.yml) [![deploy to dockerhub](https://github.com/madiele/vod2pod-rss/actions/workflows/docker-image.yml/badge.svg?branch=stable)](https://github.com/madiele/vod2pod-rss/actions/workflows/docker-image.yml)

Converts a YouTube or Twitch channel into a full-blown podcast.

<a label="example of it working with podcast addict" href="url"><img src="https://user-images.githubusercontent.com/4585690/231301791-2f838fb3-4f6e-4382-bac4-c968bfe98c08.png" align="left" height="350" ></a>

## Features:

- Completely converts the VoDs into a proper podcast RSS that can be listened to directly inside the client.
- The VoDs are not downloaded on the server, so no need for storage while self-hosting this app.
- VoDs are transcoded to MP3 192k on the fly by default, tested to be working flawlessly even on a Raspberry Pi 3-4.
- also work on standard rss podcasts feed if you want to have a lower bitrate version to save mobile data.

## Known issues:

- First time you ask for a feed it will take up to a minute or two for the request to go through, following request will be faster as the cache get build.
- Youtube channel avatar is missing (https://github.com/madiele/vod2pod-rss/issues/26)

# Usage

<a label="frontend" href="url"><img src="https://user-images.githubusercontent.com/4585690/231298488-c561974a-d4db-49ab-a971-de30054e1711.png" align="right" height="250" ></a>
Just go to the root of the server es: `myserver.com` and paste the channel you want to convert to podcast and copy the generated link.


you can also build the url manually just add `/transcodize_rss?url=channel_url` to your server path, and an RSS will be generated. Replace `channel_url` with the URL of the YouTube or Twitch channel you want to convert into a podcast.

Example youtube: `myserver.com/transcodize_rss?url=https://www.youtube.com/c/channelname`

Example twitch: `myserver.com/transcodize_rss?url=https://www.twitch.tv/channelname`

Example rss/atom feed: `myserver.com/transcodize_rss?url=https://feeds.simplecast.com/aU_RzZ7j`


Just add the link to your podcast client.

# Installation

## install with docker
(only if you use twitch) before doing anything be sure to get your SECRET and CLIENT ID from twitch
https://dev.twitch.tv/console

precompiled images are [here](https://hub.docker.com/r/madiele/vod-to-podcast/) for linux machines with arm64, amd64. other architectures will need to be compiled (docker compose will do it automatically but it's slow)

images for raspberry pis 64bit are included

### use [docker compose](https://docs.docker.com/compose/install/) with precompiled image (easiest)

`git clone https://github.com/madiele/vod2pod-rss.git`

`cd vod2pod-rss`

edit `docker-compose.yml` with your PORT, SECRET and CLIENT_ID
(in the file you will find also optional parameters like bitrate)

`nano docker-compose.yml`

save and

`sudo docker compose up -d`

#### when you want to update:

run this inside the folder with `docker-compose.yml`

`sudo docker compose pull && sudo docker compose up -d`

then run this to delete the old version form your system (note: this will also delete any other unused image you have)

`sudo docker system prune`

## Configuration

You can set the following environment variables:

- `TRANSCODE`: Set to "false" to disable transcoding, usefull if you only need the feeds (default: "true")
- `BITRATE`: Set the bitrate of the trascoded stream to your client (default: "192")
- `SUBFOLDER`: Set the the root path of the app, useful for reverse proxies (default: "/")

## Donations

This is a passion project, and mostly made for personal use, but if you want to gift a pizza margherita, feel free!

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/madiele)

# Contributing

Pull requests for small features and bugs are welcome. For major changes, please open an issue first to discuss what you would like to change.
to run locally you will need to install ffmpeg and yt-dlp, for redis just use `sudo docker compose -f docker compose.dev_enviroment.yml up -d` on the root folder of the project, set enviroment variables using `export RUST_LOG=DEBUG` you can see all the envs you need to set in the docker-compose.yml file, feel free to get in contact if you need help!
