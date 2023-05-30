# Installation

## 1. Install Docker

Please refer to the Docker installation section to see how to install Docker: [[Docker#Installation]]

## 2. Create a directory for the files

Create 3 directories for your Jellyfin server, these are `config`, `cache`, and `media`.

```Shell
mkdir /path/to/jellyfin-config
mkdir /path/to/jellyfin-cache
mkdir /path/to/jellyfin-media
```

## 3. Docker compose file

Create a file called `docker-compose.yml` and paste the following into it, editing as required:

```yml
version: '3.5'
services:
  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
    network_mode: 'host'
    volumes:
      - /path/to/jellyfin-config:/config
      - /path/to/jellyfin-cache:/cache
      - /path/to/jellyfin-media:/media
    restart: 'unless-stopped'
    # Optional - alternative address used for autodiscovery
    environment:
      - JELLYFIN_PublishedServerUrl=http://example.com
    # Optional - may be necessary for docker healthcheck to pass if running in host network mode
    extra_hosts:
      - "host.docker.internal:host-gateway"
```

## 4. Run Docker compose

```Shell
docker compose up -d
```