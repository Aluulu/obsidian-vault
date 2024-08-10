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
      - /config/radarr:/config
      - /data:/data
    restart: unless-stopped
    network_mode: "service:gluetun"


  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - UMASK=002
      - TZ=Etc/UTC
    volumes:
      - /config/sonarr:/config
      - /data:/data
    restart: unless-stopped
    network_mode: "service:gluetun"


  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    environment:
      - PUID=1000
      - PGID=1000
      - UMASK=002
      - TZ=Etc/UTC
    volumes:
      - /config/prowlarr:/config
    restart: unless-stopped
    network_mode: "service:gluetun"


  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - WEBUI_PORT=8080
      - TORRENTING_PORT=6881
    volumes:
      - /config:/config
      - /data/downloads:/data/downloads
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
      - /wireguard:/gluetun/wireguard # Have a file called wg0.conf in this directory
    ports:
    ######## qbittorrent
      - 8080:8080 # WebUI
      - 6881:6881 # tcp connection port
    ########

    ######## Radarr
      - 7878:7878
    ########

    ######## prowlarr
      - 9696:9696
    ########

    ######## sonarr
      - 8989:8989
    ########


  portainer:
    image: portainer/portainer-ce
    container_name: portainer
    command: -H unix:///var/run/docker.sock
    ports:
      - "9443:9443"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
      - "/config/portainer:/config/portainer"
    restart: always


  jellyseerr:
    image: fallenbagel/jellyseerr:latest
    container_name: jellyseerr
    environment:
      - LOG_LEVEL=debug
      - TZ=Asia/Tashkent
      - PORT=5055 #optional
    ports:
      - 5055:5055
    volumes:
      - /path/to/appdata/config:/app/config
    restart: unless-stopped
```

## Sign into Qbittorrent web portal

Go to the webportal for Qbittorrent. In my case it's port 8080, so:
`192.168.0.68:8080`

The default password for the web portal is shown in the docker logs. You can see it when attached or you can use a application like portainer to easily view logs for each container.

Once in, go to your settings by clicking the cog icon in the top bar. Then in the `Downloads` section menu, change your Default Save Path to where your root folder is. In this case it's:
`/data/downloads`

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

### Linking Prowlarr to your other apps

In your other apps like Radarr, go to `Settings` > `General` > `Copy API Key`.
Once you have the key copied, go to Prowlarr and head to `Settings` > `Apps` > `Add` > `Paste in your API key`.

## Create the file structure

You will need to create a file structure like the one down below. You can do so with the following command:

```Bash
mkdir -p data/downloads data/media/{books,movies,music,tv}
```

```
data
├── downloads
└── media
    ├── movies
    └── tv
```

This is because you will be putting your torrents and media into these folders to help organise your downloads and to make it easier for the Starr programs to communicate.
## Adding a film/series

When adding a film or series, you will need to change the root folder to:

Radarr - `/data/media/movies`

OR

Sonarr - `/data/media/tv`