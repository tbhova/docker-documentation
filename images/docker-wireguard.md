# [linuxserver/wireguard](https://github.com/linuxserver/docker-wireguard)

[![GitHub Stars](https://img.shields.io/github/stars/linuxserver/docker-wireguard.svg?style=flat-square&color=E68523&logo=github&logoColor=FFFFFF)](https://github.com/linuxserver/docker-wireguard)
[![GitHub Release](https://img.shields.io/github/release/linuxserver/docker-wireguard.svg?style=flat-square&color=E68523&logo=github&logoColor=FFFFFF)](https://github.com/linuxserver/docker-wireguard/releases)
[![GitHub Package Repository](https://img.shields.io/static/v1.svg?style=flat-square&color=E68523&label=linuxserver.io&message=GitHub%20Package&logo=github&logoColor=FFFFFF)](https://github.com/linuxserver/docker-wireguard/packages)
[![GitLab Container Registry](https://img.shields.io/static/v1.svg?style=flat-square&color=E68523&label=linuxserver.io&message=GitLab%20Registry&logo=gitlab&logoColor=FFFFFF)](https://gitlab.com/Linuxserver.io/docker-wireguard/container_registry)
[![Quay.io](https://img.shields.io/static/v1.svg?style=flat-square&color=E68523&label=linuxserver.io&message=Quay.io)](https://quay.io/repository/linuxserver.io/wireguard)
[![MicroBadger Layers](https://img.shields.io/microbadger/layers/linuxserver/wireguard.svg?style=flat-square&color=E68523)](https://microbadger.com/images/linuxserver/wireguard "Get your own version badge on microbadger.com")
[![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/wireguard.svg?style=flat-square&color=E68523&label=pulls&logo=docker&logoColor=FFFFFF)](https://hub.docker.com/r/linuxserver/wireguard)
[![Docker Stars](https://img.shields.io/docker/stars/linuxserver/wireguard.svg?style=flat-square&color=E68523&label=stars&logo=docker&logoColor=FFFFFF)](https://hub.docker.com/r/linuxserver/wireguard)
[![Build Status](https://ci.linuxserver.io/view/all/job/Docker-Pipeline-Builders/job/docker-wireguard/job/master/badge/icon?style=flat-square)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-wireguard/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/wireguard/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/wireguard/latest/index.html)

[WireGuard®](https://www.wireguard.com/) is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN. WireGuard is designed as a general purpose VPN for running on embedded interfaces and super computers alike, fit for many different circumstances. Initially released for the Linux kernel, it is now cross-platform (Windows, macOS, BSD, iOS, Android) and widely deployable. It is currently under heavy development, but already it might be regarded as the most secure, easiest to use, and simplest VPN solution in the industry.

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/).

Simply pulling `linuxserver/wireguard` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |


## Usage

Here are some example snippets to help you get started creating a container from this image.

### docker

```
docker create \
  --name=wireguard \
  --cap-add=NET_ADMIN \
  --cap-add=SYS_MODULE \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -e SERVERURL=wireguard.domain.com `#optional` \
  -e SERVERPORT=51820 `#optional` \
  -e PEERS=1 `#optional` \
  -e PEERDNS=8.8.8.8 `#optional` \
  -e INTERNAL_SUBNET=10.13.13.0 `#optional` \
  -p 51820:51820/udp \
  -v /path/to/appdata/config:/config \
  -v /lib/modules:/lib/modules \
  --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
  --restart unless-stopped \
  linuxserver/wireguard
```


### docker-compose

Compatible with docker-compose v2 schemas.

```yaml
---
version: "2.1"
services:
  wireguard:
    image: linuxserver/wireguard
    container_name: wireguard
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - SERVERURL=wireguard.domain.com #optional
      - SERVERPORT=51820 #optional
      - PEERS=1 #optional
      - PEERDNS=8.8.8.8 #optional
      - INTERNAL_SUBNET=10.13.13.0 #optional
    volumes:
      - /path/to/appdata/config:/config
      - /lib/modules:/lib/modules
    ports:
      - 51820:51820/udp
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
    restart: unless-stopped
```

## Parameters

Docker images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

### Ports (`-p`)

| Parameter | Function |
| :----: | --- |
| `51820/udp` | wireguard port |


### Environment Variables (`-e`)

| Env | Function |
| :----: | --- |
| `PUID=1000` | for UserID - see below for explanation |
| `PGID=1000` | for GroupID - see below for explanation |
| `TZ=Europe/London` | Specify a timezone to use EG Europe/London |
| `SERVERURL=wireguard.domain.com` | External IP or domain name for docker host. Used in server mode. If set to `auto`, the container will try to determine and set the external IP automatically |
| `SERVERPORT=51820` | External port for docker host. Used in server mode. |
| `PEERS=1` | Number of peers to create confs for. Required for server mode. |
| `PEERDNS=8.8.8.8` | DNS server set in peer/client configs. Used in server mode. |
| `INTERNAL_SUBNET=10.13.13.0` | Internal subnet for the wireguard and server and peers (only change if it clashes). Used in server mode. |

### Volume Mappings (`-v`)

| Volume | Function |
| :----: | --- |
| `/config` | Contains all relevant configuration files. |
| `/lib/modules` | Maps host's modules folder. |


#### Miscellaneous Options
| Parameter | Function |
| :-----:   | --- |
| `--sysctl=` | Required for client mode. |


## User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id user` as below:

```
  $ id username
    uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```

## Application Setup

This image is designed for Ubuntu and Debian x86_64 systems only. During container start, it will download the necessary kernel headers and build the kernel module (until kernel 5.6, which has the module built-in, goes mainstream).

If you're on a debian/ubuntu based host with a custom or downstream distro provided kernel (ie. Pop!_OS), the container won't be able to install the kernel headers from the regular ubuntu and debian repos. In those cases, you can try installing the headers on the host via `sudo apt install linux-headers-$(uname -r)` (if distro version) and then add a volume mapping for `/usr/src:/usr/src`, or if custom built, map the location of the existing headers to allow the container to use host installed headers to build the kernel module (tested successful on Pop!_OS, ymmv).

This can be run as a server or a client, based on the parameters used. 

## Server Mode
If the environment variable `PEERS` is set to a number, the container will run in server mode and the necessary server and peer/client confs will be generated. The peer/client config qr codes will be output in the docker log. They will also be saved in text and png format under `/config/peerX`.

Variables `SERVERURL`, `SERVERPORT`, `INTERNAL_SUBNET` and `PEERDNS` are optional variables used for server mode. Any changes to these environment variables will trigger regeneration of server and peer confs. Peer/client confs will be recreated with existing private/public keys. Delete the peer folders for the keys to be recreated along with the confs.

To add more peers/clients later on, you can run `docker exec -it wireguard /app/add-peer` while the container is running.

To display the QR codes of active peers again, you can use the following command and list the peer numbers as arguments: `docker exec -it wireguard /app/show-peer 1 4 5` (Keep in mind that the QR codes are also stored as PNGs in the config folder).

The templates used for server and peer confs are saved under `/config/templates`. Advanced users can modify these templates and force conf generation by deleting `/config/wg0.conf` and restarting the container.

## Client Mode
Do not set the `PEERS` environment variable. Drop your client conf into the config folder as `/config/wg0.conf` and start the container. 


## Docker Mods
[![Docker Mods](https://img.shields.io/badge/dynamic/yaml?style=for-the-badge&color=E68523&label=mods&query=%24.mods%5B%27wireguard%27%5D.mod_count&url=https%3A%2F%2Fraw.githubusercontent.com%2Flinuxserver%2Fdocker-mods%2Fmaster%2Fmod-list.yml)](https://mods.linuxserver.io/?mod=wireguard "view available mods for this container.")

We publish various [Docker Mods](https://github.com/linuxserver/docker-mods) to enable additional functionality within the containers. The list of Mods available for this image (if any) can be accessed via the dynamic badge above.


## Support Info

* Shell access whilst the container is running:
  * `docker exec -it wireguard /bin/bash`
* To monitor the logs of the container in realtime:
  * `docker logs -f wireguard`
* Container version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' wireguard`
* Image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/wireguard`

## Versions

* **05.04.20:** - Add `INTERNAL_SUBNET` variable to prevent subnet clashes. Add templates for server and peer confs.
* **01.04.20:** - Add `show-peer` script and include info on host installed headers.
* **31.03.20:** - Initial Release.