# docker-rtl-tcp
#### Dockerized RTL-SDR server for Raspberry Pi and other Devices

This Dockerfile allows you to easily deploy a native RTL-SDR server using Docker

# Tags

| Image | Tag | Build |
|:------------------:|:--------------:|:-----------------:|
| ghcr.io/lizenzfass78851/docker-rtl-tcp | master | [![Build and Publish Docker Image](https://github.com/LizenzFass78851/docker-rtl-tcp/actions/workflows/docker-image.yml/badge.svg?branch=master)](https://github.com/LizenzFass78851/docker-rtl-tcp/actions/workflows/docker-image.yml) |

# Instructions

- It's needed to block certain kernel modules to get the RTL-SDR device work properly.
   Place run the folowing command to create the `blacklist-rtl.conf` in your host `/lib/modprobe.d/` folder:
```bash
echo -e 'blacklist dvb_usb_rtl28xxu\nblacklist rtl2832\nblacklist rtl2830' | tee /lib/modprobe.d/blacklist-rtl.conf
```

<details>
  <summary>optional: if the container does not work</summary>

- Additionally, we need to set the correct permissions to the modules for allowing TCP messages. Place the `rtl_sdr.rules` file into your host `/etc/udev/rules.d/` folder:
```bash
GIT_REPO_AND_BRANCH=LizenzFass78851/docker-rtl-tcp/master
curl https://raw.githubusercontent.com/$GIT_REPO_AND_BRANCH/files/rtl_sdr.rules \
  -o /etc/udev/rules.d/rtl_sdr.rules
```

</details>

- Reboot your Device, this should only have to be done once.

- Run the container using the `docker compose` command included here:
```bash
mkdir ./rtl-tcp && cd ./rtl-tcp
GIT_REPO_AND_BRANCH=LizenzFass78851/docker-rtl-tcp/master
curl https://raw.githubusercontent.com/$GIT_REPO_AND_BRANCH/docker-compose.yml \
  -o ./docker-compose.yml
docker compose up -d
```

- The server is listening on port `1234`, connect to it using your RTL-SDR client (i.e. SDRSharp) and the IP of the Device.
```
sdr://<your-hostname>:1234
```

- Enjoy and open an issue or collaborate with a PR if anything is not working as expected.
