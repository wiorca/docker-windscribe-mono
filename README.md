# Docker Windscribe Mono Image

A docker image with the latest version of mono and windscribe installed.

## Deprecated

I do plan to keep updating this, but I would recommend using other images with the base docker-windscribe client instead.  You can then use the network of the windscribe container.

--net=container:[docker-windscribe-continer-name]

## About the image

Windscribe docker container that provides mono packages.

## Usage

Here are some example snippets to help you get started creating a container.

### docker

```
docker create \
  --name=docker-windscribe-mono \
  -e TZ=America/New_York \
  -e WINDSCRIBE_USERNAME=username \
  -e WINDSCRIBE_PASSWORD=password \
  -e WINDSCRIBE_PROTOCOL=stealth \
  -e WINDSCRIBE_PORT=80 \
  -e WINDSCRIBE_PORT_FORWARD=9999 \
  -e WINDSCRIBE_LOCATION=US \
  -e WINDSCRIBE_LANBYPASS=on \
  -e WINDSCRIBE_FIREWALL=on \
  -v /location/on/host:/config \
  --dns 8.8.8.8 \
  --cap-add NET_ADMIN \
  --restart unless-stopped \
  wiorca/docker-windscribe-mono
```


### docker-compose

Compatible with docker-compose schemas.

```
---
version: "2.1"
services:
  docker-windscribe-mono:
    image: wiorca/docker-windscribe-mono
    container_name: docker-windscribe-mono
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/New_York
      - WINDSCRIBE_USERNAME=username
      - WINDSCRIBE_PASSWORD=password
      - WINDSCRIBE_PROTOCOL=stealth
      - WINDSCRIBE_PORT=80
      - WINDSCRIBE_PORT_FORWARD=9999
      - WINDSCRIBE_LOCATION=US
      - WINDSCRIBE_LANBYPASS=on
      - WINDSCRIBE_FIREWALL=on
    volumes:
      - /location/on/host:/config
    dns:
      - 8.8.8.8
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above).

This docker container doesn't define any new configuration parameters.  The configuration of qbittorrent is achieved through files.

The following parameters are inhereted from the parent container:

| Parameter | Examples/Options | Function |
| :----: | --- | --- |
| PUID | 1000 | The nummeric user ID to run the application as, and assign to the user docker_user |
| PGID | 1000 | The numeric group ID to run the application as, and assign to the group docker_group |
| TZ=Europe/London | The timezone to run the container in |
| WINDSCRIBE_USERNAME | username | The username used to connect to windscribe |
| WINDSCRIBE_PASSWORD | password | The password associated with the username |
| WINDSCRIBE_PROTOCOL | stealth OR tcp OR udp | The protocol to use when connecting to windscribe, which must be on 'windscribe protocol' list |
| WINDSCRIBE_PORT | 443, 80, 53 | The port to connect to windscribe over, which must be on 'windscribe port' list for that protocol |
| WINDSCRIBE_PORT_FORWARD | 9898 | The port you have convigured to forward via windscribe. Not used by this container, but made available |
| WINDSCRIBE_LOCATION | US | The location to connect to, which must be on 'windscribe location' list |
| WINDSCRIBE_LANBYPASS | on, off | Allow other applications on the docker bridge to connect to this container if on |
| WINDSCRIBE_FIREWALL | on, off | Enable the windscribe firewall if on, which is recommended. |

## Volumes

| Volume | Example | Function |
| :----: | --- | --- |
| /config | /opt/docker/docker-windscribe-sonarr-config | The home directory of docker_user, and where configuration files will live (inhereted from parent)|

## Below are the instructions for updating containers:

### Via Docker Run/Create
* Update the image: `docker pull wiorca/docker-windscribe-mono`
* Stop the running container: `docker stop docker-windscribe-mono`
* Delete the container: `docker rm docker-windscribe-mono`
* Recreate a new container with the same docker create parameters as instructed above
* Start the new container: `docker start docker-windscribe-mono`
* You can also remove the old dangling images: `docker image prune`

### Via Docker Compose
* Update all images: `docker-compose pull`
  * or update a single image: `docker-compose pull docker-windscribe-mono`
* Let compose update all containers as necessary: `docker-compose up -d`
  * or update a single container: `docker-compose up -d docker-windscribe-mono`
* You can also remove the old dangling images: `docker image prune`

### Via Watchtower auto-updater (especially useful if you don't remember the original parameters)
* Pull the latest image at its tag and replace it with the same env variables in one run:
  ```
  docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --run-once docker-windscribe-mono
  ```
* You can also remove the old dangling images: `docker image prune`

## Building locally

If you want to make local modifications to these images for development purposes or just to customize the logic:
```
git clone https://github.com/wiorca/docker-windscribe-mono.git
cd docker-windscribe-mono
docker build \
  --no-cache \
  --pull \
  -t wiorca/docker-windscribe-mono:latest .
```
