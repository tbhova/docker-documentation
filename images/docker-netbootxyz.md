# [linuxserver/netbootxyz](https://github.com/linuxserver/docker-netbootxyz)

[![GitHub Release](https://img.shields.io/github/release/linuxserver/docker-netbootxyz.svg?style=flat-square&color=E68523)](https://github.com/linuxserver/docker-netbootxyz/releases)
[![MicroBadger Layers](https://img.shields.io/microbadger/layers/linuxserver/netbootxyz.svg?style=flat-square&color=E68523)](https://microbadger.com/images/linuxserver/netbootxyz "Get your own version badge on microbadger.com")
[![MicroBadger Size](https://img.shields.io/microbadger/image-size/linuxserver/netbootxyz.svg?style=flat-square&color=E68523)](https://microbadger.com/images/linuxserver/netbootxyz "Get your own version badge on microbadger.com")
[![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/netbootxyz.svg?style=flat-square&color=E68523)](https://hub.docker.com/r/linuxserver/netbootxyz)
[![Docker Stars](https://img.shields.io/docker/stars/linuxserver/netbootxyz.svg?style=flat-square&color=E68523)](https://hub.docker.com/r/linuxserver/netbootxyz)
[![Build Status](https://ci.linuxserver.io/view/all/job/Docker-Pipeline-Builders/job/docker-netbootxyz/job/master/badge/icon?style=flat-square)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-netbootxyz/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/netbootxyz/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/netbootxyz/latest/index.html)

[Netbootxyz](https://netboot.xyz) is a way to PXE boot various operating system installers or utilities from one place within the BIOS without the need of having to go retrieve the media to run the tool. iPXE is used to provide a user friendly menu from within the BIOS that lets you easily choose the operating system you want along with any specific types of versions or bootable flags.

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/).

Simply pulling `linuxserver/netbootxyz` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v7-latest |


## Usage

Here are some example snippets to help you get started creating a container from this image.

### docker

```
docker create \
  --name=netbootxyz \
  -p 69:69/udp \
  --restart unless-stopped \
  linuxserver/netbootxyz
```


### docker-compose

Compatible with docker-compose v2 schemas.

```yaml
---
version: "2"
services:
  netbootxyz:
    image: linuxserver/netbootxyz
    container_name: netbootxyz
    ports:
      - 69:69/udp
    restart: unless-stopped
```

## Parameters

Docker images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

### Ports (`-p`)

| Parameter | Function |
| :----: | --- |
| `69/udp` | TFTP Port. |


### Environment Variables (`-e`)

| Env | Function |
| :----: | --- |

### Volume Mappings (`-v`)

| Volume | Function |
| :----: | --- |



## Application Setup

To use this image you need an existing DHCP server where you can set this TFTP server as your DHCP boot destination. This image does not contain a DHCP server nor do we aim to support one in the future. This is simply a TFTP server hosting the latest IPXE kernel builds from [netboot.xyz](https://netboot.xyz). If you are interested in their project and lack the ability to setup a DHCP server to boot this payload they also have USB stick images you can use available on their [downloads page](https://netboot.xyz/downloads/).

### Router Setup Examples

#### PFSense/OPNsense
Services -> DHCP Server

Set both the option for "TFTP Server" and the options under the Advanced "Network Booting" section. 
* check enable
* Next server- IP used for TFTP Server
* Default BIOS file name- `netboot.xyz.kpxe`
* UEFI 32 bit file name- `netboot.xyz.efi`
* UEFI 64 bit file name- `netboot.xyz.efi`

#### DD-WRT
Administration->Services -> Additional DNSMasq Options
Set the following lines: 
```
dhcp-match=set:bios,60,PXEClient:Arch:00000
dhcp-boot=tag:bios,netboot.xyz.kpxe,,YOURSERVERIP
dhcp-match=set:efi32,60,PXEClient:Arch:00006
dhcp-boot=tag:efi32,netboot.xyz.efi,,YOURSERVERIP
dhcp-match=set:efi64,60,PXEClient:Arch:00009
dhcp-boot=tag:efi64,netboot.xyz.efi,,YOURSERVERIP 
```

#### Tomato
Advanced -> DHCP/DNS -> Dnsmasq Custom configuration
Set the following lines: 
```
dhcp-match=set:bios,60,PXEClient:Arch:00000
dhcp-boot=tag:bios,netboot.xyz.kpxe,,YOURSERVERIP
dhcp-match=set:efi32,60,PXEClient:Arch:00006
dhcp-boot=tag:efi32,netboot.xyz.efi,,YOURSERVERIP
dhcp-match=set:efi64,60,PXEClient:Arch:00009
dhcp-boot=tag:efi64,netboot.xyz.efi,,YOURSERVERIP 
```

#### OpenWRT
```
uci set dhcp.@dnsmasq[0].dhcp_match=set:bios,60,PXEClient:Arch:00000
uci set dhcp.@dnsmasq[0].dhcp_boot=tag:bios,netboot.xyz.kpxe,,YOURSERVERIP
uci set dhcp.@dnsmasq[0].dhcp_match=set:efi32,60,PXEClient:Arch:00006
uci set dhcp.@dnsmasq[0].dhcp_boot=tag:efi32,netboot.xyz.efi,,YOURSERVERIP
uci set dhcp.@dnsmasq[0].dhcp_match=set:efi64,60,PXEClient:Arch:00009
uci set dhcp.@dnsmasq[0].dhcp_boot=tag:efi64,netboot.xyz.efi,,YOURSERVERIP
uci commit
/etc/init.d/dnsmasq restart
```

Anything else from a router standpoint is a crapshoot for supporting Dnsmasq options or proprietary PXE boot options, check Google for support (try your exact router model number with 'pxe boot') or look into setting up your own DHCP server in Linux.

This image also contains `netboot.xyz.efi` which can be used to boot using UEFI network boot. The UEFI boot and menu will have limited functionality if you choose to use it. 



## Support Info

* Shell access whilst the container is running:
  * `docker exec -it netbootxyz /bin/bash`
* To monitor the logs of the container in realtime:
  * `docker logs -f netbootxyz`
* Container version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' netbootxyz`
* Image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/netbootxyz`

## Versions

* **22.10.19:** - Initial release.