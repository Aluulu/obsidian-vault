
> [!Info] Info
> Don't know what server to start? Check out the types of servers you can run and what their benefits and negatives are here: [[#Differences in Servers]]

# Setting up Vanilla server (No mods)

## Set up

### 1. Install packages

```Shell
sudo apt update
sudo apt install screen default-jdk
```

- `screen` is for running the Minecraft server in the background ^384952
- `default-jdk` is a Java package that Minecraft needs in order to run

### 2. Create a Minecraft user

It’s best practice to let the Minecraft server run under its own dedicated account, rather than using root or some other account. Create a new account in your server with the following command:

```Shell
sudo useradd -m -r -d /opt/minecraft minecraft
```

This will create a new user called "minecraft". It sets its home directory to be `/opt/minecraft` and it creates the account as a system account.

### 3. Create a folder to host your world

Each server instance we run will need its own directory under the `/opt/minecraft` directory. For this first server instance, let’s call it `survival` and create the following directory:

```Shell
sudo mkdir /opt/minecraft/survival
```

### 4. Download and move your server java file

Download your preferred Minecraft version for your server or download the latest version here: 
https://www.minecraft.net/en-us/download/server

Then, move it into the root directory of your minecraft world, like so:

```Shell
sudo cp ~/server.jar /opt/minecraft/survival
```

### 5. Create a eula.txt file and set it as true

Minecraft servers require a eula file to be true before it runs. To do this, simply create a file called `eula.txt` and inside of it write `eula=true`. Or type this command:

```Shell
sudo bash -c "echo eula=true > /opt/minecraft/survival/eula.txt"
```

### 6. Give your minecraft account ownership to the folder

Provide your Minecraft account that is running the server ownership to all files.

```Shell
sudo chown -R minecraft /opt/minecraft/survival/
```

>[!Warning]
>When the server creates files, you will not have access to them. To solve this, run the following command:
>```Shell
>sudo chmod -R o+rwx /opt/minecraft/survival/

## SystemD service
### 1. Create a systemd service

```Shell
sudo nano /etc/systemd/system/minecraft@.service
```

Paste the following into that file:

```systemd
[Unit]
Description=Minecraft Server: %i
After=network.target

[Service]
WorkingDirectory=/opt/minecraft/%i

User=minecraft
Group=minecraft

Restart=always

ExecStart=/usr/bin/screen -DmS mc-%i /usr/bin/java -Xmx4G -jar minecraft_server.jar nogui

ExecStop=/usr/bin/screen -p 0 -S mc-%i -X eval 'stuff "say SERVER SHUTTING DOWN IN 5 SECONDS. SAVING ALL MAPS..."\015'
ExecStop=/bin/sleep 5
ExecStop=/usr/bin/screen -p 0 -S mc-%i -X eval 'stuff "save-all"\015'
ExecStop=/usr/bin/screen -p 0 -S mc-%i -X eval 'stuff "stop"\015'


[Install]
WantedBy=multi-user.target
```

>[!Tip]
>You can change the amount of RAM the server uses by changing the following line
>"-Xmx**4G**" to whatever you wish. Examples include:
>"-Xmx**1G**" - 1 GigaByte of RAM
>"-Xmx**2G**" - 2 GigaByte of RAM
>"-Xmx**8G**" - 8 GigaByte of RAM

### 2. Start the systemd service

Type the following to start your service

```Shell
sudo systemctl start minecraft@survival
```

To check the status of your server, run the following command:

```Shell
sudo systemctl status minecraft@survival
```

To make your server start on startup, run the following command:

```Shell
sudo systemctl enable minecraft@survival
```

### 3. Connect your terminal to the Minecraft terminal

If you wish to connect to your server, use the following command:

```Shell
sudo -u minecraft screen -r
```

It is a good idea to give yourself OP after connecting yourself to the terminal by putting in the following command:

```Shell
op [username]
```

# Setting up a Forge server (Mods and Plugins)

## 1. Complete the Vanilla server set up

To set up Forge, you must first have a working Vanilla server working. To do if you haven't already, go to [[#Setting up Vanilla server (No mods)]].

## 2. Download Forge

Download the version of Forge you wish to install from the following page:
https://files.minecraftforge.net/net/minecraftforge/forge/

Afterwards, put it in the location of your minecraft server like so:

```Shell
cp forge-x.xx.x-installer.jar /opt/minecraft/survival
```

Then, run the following command and adjust it based on your version.

```Shell
java -jar forge-x.xx.x-installer.jar --installServer
```
## Edit your systemd service

Type the following command to edit your systemd service:

```Shell
sudo nano /etc/systemd/system/minecraft@.service
```

Change the ExecStart to this line:

>[!Important]
>Make sure you change the name of the server executable. It may be something like forge-1.20.4-49.0.30-shim.jar.

```systemd
[Unit]
Description=Minecraft Server: %i
After=network.target

[Service]
WorkingDirectory=/opt/minecraft/%i

User=minecraft
Group=minecraft

Restart=always

ExecStart=/usr/bin/screen -DmS mc-%i /usr/bin/java -Xmx4G -Xmx4G -jar FORGE_FILE_NAME.jar --nogui

ExecStop=/usr/bin/screen -p 0 -S mc-%i -X eval 'stuff "say SERVER SHUTTING DOWN IN 5 SECONDS. SAVING ALL MAPS..."\015'
ExecStop=/bin/sleep 5
ExecStop=/usr/bin/screen -p 0 -S mc-%i -X eval 'stuff "save-all"\015'
ExecStop=/usr/bin/screen -p 0 -S mc-%i -X eval 'stuff "stop"\015'


[Install]
WantedBy=multi-user.target
```

### 2. Restart the systemd service

Type the following to start your service to ensure your server is working as intended:

```Shell
sudo systemctl restart minecraft@survival
```

To check the status of your server, run the following command:

```Shell
sudo systemctl status minecraft@survival
```

### 3. Install mods

Server mods are placed into the `mods` folder located at `/opt/minecraft/survival/mods`. If it does not exist, you can create it yourself:

```Shell
mkdir /opt/minecraft/survival/mods
```

Any of the Forge mods will work as long as they are compatible with the correct version.

>[!Important]
>Both the client and the server must have the same mods installed for them to work correctly.

Now restart your server again after updating the mods:

```Shell
sudo systemctl restart minecraft@survival
```

# Setting up a Paper server

Paper (formerly known as PaperSpigot, distributed via the Paperclip patch utility) is a high performance fork of Spigot. The goal of PaperSpigot is to basically make every darn thing configurable. Paper adds over 200 patches to Spigot and its API which because of this is known to cause some incompatibilities with certain plugins

## Set up

Follow the steps of setting up a Vanilla minecraft server here with 2 key differences [[#Setting up Vanilla server (No mods)]]

### Paper download

Instead of downloading the Minecraft server file directly from Mojang, instead you will use the file located here:
https://papermc.io/downloads/paper

>[!Tip]
>Paper is compatible with both Spigot and Bukkit plugins. It's okay if a plugin does not explicitly mention Paper compatibility. It'll still work.

You can check if your plugins are loaded by typing `/plugins`  in-game or `plugins` into your server console.
### systemd differences

Instead of using the Vanilla `ExecStart` arguments, use this slightly different one designed with use for Paper. Remember to change the RAM to your liking:

```systemd
ExecStart=/usr/bin/screen -DmS mc-%i /usr/bin/java -Xmx4096M -Xms4096M -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+ParallelRefProcEnabled -XX:+PerfDisableSharedMem -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:G1HeapRegionSize=8M -XX:G1HeapWastePercent=5 -XX:G1MaxNewSizePercent=40 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1NewSizePercent=30 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:G1ReservePercent=20 -XX:InitiatingHeapOccupancyPercent=15 -XX:MaxGCPauseMillis=200 -XX:MaxTenuringThreshold=1 -XX:SurvivorRatio=32 -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true -jar server.jar --nogui
```

## Paper Plugins

In order to use certain plugins for Paper, you must first create a folder in your Minecraft worlds root directory called `plugins`.

```Shell
sudo mkdir /opt/minecraft/survival/plugins
```

There you can put your plugins that you wish to install. You may wish to use the official Paper plugin repository to find compatible mods:
https://hangar.papermc.io/


# Server configuration
## server.properties

Minecraft has a properties file that determines certain settings for your server. If there is not one available on startup, one is created.

You may find the complete up-to-date version here:
https://minecraft.wiki/w/Server.properties

```TOML
#Minecraft server properties

# Allows users to use flight on the server while in Survival mode, if they have a mod that provides flight installed.
# [false/true]
allow-flight=false

# Allows players to travel to the Nether.
# [true/false]
allow-nether=true

# Send console command outputs to all online operators.
# [true/false]
broadcast-console-to-ops=true

# Send rcon console command outputs to all online operators.
# [true/false]
broadcast-rcon-to-ops=true

# Defines the difficulty (such as damage dealt by mobs and the way hunger and poison affects players) of the server.
# [peaceful/easy/normal/hard]
difficulty=easy

# Enables command blocks
# [false/true]
enable-command-block=false

# Exposes an MBean with the Object name net.minecraft.server:type=Server and two attributes averageTickTime and tickTimes exposing the tick times in milliseconds. 
# [false/true]
enable-jmx-monitoring=false

# Enables GameSpy4 protocol server listener. Used to get information about server. 
# [false/true]
enable-query=false

# Enables remote access to the server console. 
# [false/true]
enable-rcon=false

# Makes the server appear as "online" on the server list. If set to false, it will suppress replies from clients. This means it will appear as offline, but will still accept connections. 
# [true/false]
enable-status=true

# If set to true, players without a Mojang-signed public key will not be able to connect to the server. 
# [true/false]
enforce-secure-profile=true

# Enforces the whitelist on the server. 
# [false/true]
enforce-whitelist=false

# Controls how close entities need to be before being sent to clients. Higher values means they'll be rendered from farther away, potentially causing more lag
# [10 - 10000]
entity-broadcast-range-percentage=100

# Force players to join in the default game mode. 
# [false/true]
force-gamemode=false

# Sets the default permission level for functions. 
# [1-4]
function-permission-level=2

# Defines the mode of gameplay. 
# [survival/creative/adventure/spectator]
gamemode=survival

# Defines whether structures (such as villages) can be generated. Note: Dungeons still generate if this is set to false. 
# [true/false]
generate-structures=true

# The settings used to customize world generation. Follow its format and write the corresponding JSON string. Remember to escape all : with \:. This only applies to generation when the level-type is minecraft:flat
generator-settings={}

# If set to true, server difficulty is ignored and set to hard and players are set to spectator mode if they die. 
# [false/true]
hardcore=false

# If set to true, a player list is not sent on status requests. 
# [false/true]
hide-online-players=false

# Comma-separated list of datapacks to not be auto-enabled on world creation. 
initial-disabled-packs=

# Comma-separated list of datapacks to be enabled during world creation. Feature packs need to be explicitly enabled. 
# [vanilla]
initial-enabled-packs=vanilla

# The "level-name" value is used as the world name and its folder name. The player may also copy their saved game folder here, and change the name to the same as that folder's to load it instead.
level-name=world

# Sets a world seed for the player's world, as in Singleplayer. The world generates with a random seed if left blank. 
level-seed=

# Determines the world preset that is generated. 
# [minecraft:normal/minecraft:flat/minecraft:large_biomes/minecraft:amplified/minecraft:single_biome_surface/buffet/default_1_1/customized]
level-type=minecraft\:normal

# If set to false client IP addresses will not be shown in log messages printed to the server console or the log file. 
# [true/false]
log-ips=true

# Limiting the amount of consecutive neighbor updates before skipping additional ones. Negative values remove the limit. 
[ -1 - 1000000] [Default is 1000000]
max-chained-neighbor-updates=1000000

# The maximum number of players that can play on the server at the same time
# [0 - 2147483647] [Default is 20]
max-players=20

# The maximum number of milliseconds a single tick may take before the server watchdog stops the server with the message
# [-1 - 9223372036854775807] [Default is 60000]
max-tick-time=60000

# This sets the maximum possible size in blocks, expressed as a radius, that the world border can obtain
# [1 - 29999984] [Default is 29999984]
max-world-size=29999984

# This is the message that is displayed in the server list of the client, below the name. 
motd="A Minecraft Server"


network-compression-threshold=256

# Server checks connecting players against Minecraft account database. Set this to false only if the player's server is not connected to the Internet.
# [true/false]
online-mode=true

# Sets the default permission level for ops when using /op.
# [0 - 4] [Default is 4]
op-permission-level=4

# If non-zero, players are kicked from the server if they are idle for more than that many minutes.
# [0 - Infinity] [Default is 0]
player-idle-timeout=0

# If the ISP/AS sent from the server is different from the one from Mojang Studios' authentication server, the player is kicked. 
# [false/true]
prevent-proxy-connections=false

# Enable PvP on the server. Players shooting themselves with arrows receive damage only if PvP is enabled. 
# [true/false]
pvp=true

# Sets the port for the query server (see enable-query).
query.port=25565

# Sets the maximum amount of packets a user can send before getting kicked. Setting to 0 disables this feature.
rate-limit=0

# Sets the password for RCON: a remote console protocol that can allow other applications to connect and interact with a Minecraft server over the internet. 
rcon.password=

# Sets the RCON network port. 
rcon.port=25575

# When this option is enabled (set to true), players will be prompted for a response and will be disconnected if they decline the required pack. 
# [false/true]
require-resource-pack=false

# Optional URI to a resource pack. The player may choose to use it.
resource-pack=

# Optional UUID for the resource pack set by resource-pack to identify the pack with clients. 
resource-pack-id=

# Optional, adds a custom message to be shown on resource pack prompt when require-resource-pack is used. Expects chat component syntax, can contain multiple lines. 
resource-pack-prompt=

#  Optional SHA-1 digest of the resource pack, in lowercase hexadecimal. It is recommended to specify this, because it is used to verify the integrity of the resource pack. 
resource-pack-sha1=

# The player should set this if they want the server to bind to a particular IP. It is strongly recommended that the player leaves server-ip blank. 
server-ip=

# Changes the port the server is hosting (listening) on. This port must be forwarded if the server is hosted in a network using NAT (if the player has a home router/firewall). 
server-port=25565

# Sets the maximum distance from players that living entities may be located in order to be updated by the server, measured in chunks in each direction of the player (radius, not diameter). If entities are outside of this radius, then they will not be ticked by the server nor will they be visible to players.
# [3 - 32] [Default is 10]
simulation-distance=10

# Determines if animals can spawn. 
# [true/false]
spawn-animals=true

# Determines if monsters can spawn.
# [true/false]
spawn-monsters=true

# Determines whether villagers can spawn.
# [true/false]
spawn-npcs=true

# Determines the side length of the square spawn protection area as 2x+1. Setting this to 0 disables the spawn protection. A value of 1 protects a 3×3 square centered on the spawn point. 2 protects 5×5, 3 protects 7×7, etc. This option is not generated on the first server start and appears when the first player joins. If there are no ops set on the server, the spawn protection is disabled automatically as well.
# [0 - Infinity] [Default is 16]
spawn-protection=2

# Enables synchronous chunk writes. 
# [true/false]
sync-chunk-writes=true

text-filtering-config=

# Linux server performance improvements: optimized packet sending/receiving on Linux
# [true/false]
use-native-transport=true

# Sets the amount of world data the server sends the client, measured in chunks in each direction of the player (radius, not diameter). It determines the server-side viewing distance. 
# [3 - 32] [Default is 10]
view-distance=10

# Enables a whitelist on the server.
# [false/true]
white-list=false
```

# Making your server public

In order to make your server public, you must first enable port forwarding on your home router or following the [[Nginx]] guide to set up a reverse proxy.

You can do this by going to your terminal and typing into the following commands:

- Linux - `ping _gateway` - This will print out the IP address of your router.
- Windows - `ipconfig` - Find the IP address labelled `Default Gateway`

Go to that IP address in any browser and it will bring up the admin page for your router. Log into it and find the section dedicated to `port forwarding`.

Your server's configuration file will have a setting called `server-port`. Open that port on your router and save.

# Differences in Servers

There are many different variations of Minecraft server software that can be ran, each with their own benefits and negatives. You can decide which one to pick when you are first setting up your server.

## Vanilla

The Vanilla software is the original, untouched, unmodified Minecraft server software created and distributed directly by Mojang. This will work with just about any version of Minecraft. However, because it also comes with a plethora of bugs and a lack of configuration options.

Vanilla can be found here: https://minecraft.net/en-us/download/server
## Bukkit

Bukket is a modified version of the Vanilla server that enables a plugin API, meaning that certain modifications can be made to the game with simple drag and drop plugins. Bukkit also includes a small amount of bug fixes not seen in the Vanilla server.

You can build the latest version of Bukkit here: https://www.spigotmc.org/threads/buildtools-updates-information.42865/

## Spigot

Spigot is a modified version of CraftBukkit with a large number of improvements and optimizations, making it a better experience overall. However just like with Bukkit, you cannot download it, only build it.

You can build the latest version here: https://www.spigotmc.org/wiki/buildtools/

## Forge

Forge is a well down alternative to Minecraft that allow you to download mods that make direct modifications to Minecaft, allowing for a completely different experience. The difference between Forge and other servers like Bukkit, is that Forge Mods are direct modifications to the Minecraft program code while Bukkit Plugins are modifications that use the already-coded Minecraft properties to perform certain functions. So Forge can add completely new features, while Bukkit can only modify or tweak Minecraft features.

You can download Forge here: https://files.minecraftforge.net/net/minecraftforge/forge/

## Fabric

Fabric is also an mod loader like Forge is with some improvements. It's lightweight and faster than Forge, giving it a direct advantage, however it does not come with the history of mods that Forge has. If you are looking between the two, it may be worth deciding what mods you want, and if there are versions for both Forge and Fabric, Fabric is the ideal version.

Fabric can be downloaded here: https://fabricmc.net/use/installer/

## Paper

Paper (formerly known as PaperSpigot, distributed via the Paperclip patch utility) is a high performance fork of Spigot. The goal of Paper is to basically make everything configurable. Paper adds over 200 patches to Spigot and its API which because of this is known to cause some incompatibilities with certain plugins.

You can download Paper here: https://papermc.io/downloads

## Glowstone

Glowstone is another high performance software which prides itself on being an original project. Glowstone does not use ***any*** of Mojang's Minecraft code. It does still have the ability however, to run Bukkit plugins. Since Glowstone doesn't use any of the original Minecraft code, it is known to have some incompatibilities with plugins.

Glowstone can be found at https://www.glowstone.net/