# Docker Compose

The following is a compose.yaml file that includes radarr, sonarr, prowlarr, deluge, and gluetun VPN.

```yaml
services:
  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - UMASK=002
      - TZ=Etc/UTC
    volumes:
      - /data/radarr:/config
      - /data:/data
    restart: unless-stopped
    network_mode: "service:gluetun"


  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /data/sonarr:/config
      - /data:/data
    restart: unless-stopped
    network_mode: "service:gluetun"


  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /config/prowlarr:/config
    restart: unless-stopped
    network_mode: "service:gluetun"


  deluge: # Remember to sign into deluge from the default web portal (8112). Default password is "deluge"
    image: lscr.io/linuxserver/deluge:latest
    container_name: deluge
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - DELUGE_LOGLEVEL=error #optional
    volumes:
      - /data/deluge:/config
      - /data/torrents:/data/torrents
    restart: unless-stopped
    network_mode: "service:gluetun"
    depends_on:
      - gluetun


  gluetun:
    image: qmcgaw/gluetun
    container_name: gluetun
    cap_add:
      - NET_ADMIN
    environment:
      - VPN_SERVICE_PROVIDER=custom
      - VPN_TYPE=wireguard
    volumes:
      - /wireguard/wg0.conf:/gluetun/wireguard/wg0.conf
    ports:
    ##### DELUGE
      - 8112:8112 # Default web portal
      - 6881:6881
      - 6881:6881/udp
      - 58846:58846 #optional
    ######## Radarr
      - 7878:7878
    ########

    ######## prowlarr
      - 9696:9696
    ########

    ######## sonarr
      - 8989:8989
    ########
```

## Sign into Deluge web portal

Go to the webportal for Deluge. In my case it's port 8112, so:
`192.168.0.68:8112`

The default password for the web portal is `deluge`.

Once in. go to Preferences > Plugins > enable Label.

In Downloads, change `Download to:` into the following:

`/data/torrents`

## Set up your gluetun VPN

Place your wireguard VPN into the volume exactly as it is in the compose file. Make sure it is named `wg0.conf`

## Set up Radarr & Sonarr

Go to the webportal for Radarr and Sonarr. In my case it's port `7878` for Radarr and `8989` for Sonarr, so:
`192.168.0.X:7878` - Radarr
`192.168.0.X:8989` - Sonarr

Go to Download Clients and click the + symbol to add a client. Click Deluge, and change the password to whatever you may have set it to. If you didn't change it, the default password is `deluge`
## Set up Prowlarr

Set up a torrent list by going to `Indexers` > `Add Indexer`.

Add your favourite websites, test them, and save them.

## Create the file structure

You will need to create a file structure like the one down below. You can do so with the following command:

```Bash
mkdir -p data/torrents/{books,movies,music,tv} data/usenet/{incomplete,complete/{books,movies,music,tv}} data/media/{books,movies,music,tv}
```

```
data
├── torrents
│   ├── books
│   ├── movies
│   ├── music
│   └── tv
├── usenet
│   ├── incomplete
│   └── complete
│       ├── books
│       ├── movies
│       ├── music
│       └── tv
└── media
    ├── books
    ├── movies
    ├── music
    └── tv
```

This is because you will be putting your torrents and media into these folders to help organise your downloads and to make it easier for the Starr programs to communicate.
## Adding a film/series

When adding a film or series, you will need to change the root folder to:

Radarr - `/data/media/movies`

OR

Sonarr - `/data/media/tv`