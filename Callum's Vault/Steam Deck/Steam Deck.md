The Steam Deck is a [[Linux]] based portable handheld game system developed and manufactured by Valve. It is a modified version of [[Arch]] Linux with a [[KDE]] desktop environment.

## Game locations

### Steam game locations

Steam games are stored in the following directory:
`~/.local/share/Steam/steamapps/common`

### [[EmuDeck]] game locations

Emudeck games are stored in the following directory:
`~/Emulation/roms/<game_system_name>`

## Enable SSH

Desktop Mode:

Konsole [[KDE#Konsole]]


### On Deck:

Boot into Desktop mode and open the desktop script called SSH.sh

This will automatically grab the IP address of the Deck, and enable SSH for the session.

> [!important] Important
> SSH will only be active until a shutdown or restart

Take note of the IP address that appears. Now move to your computer/laptop.

### On Computer:

Type the following into the terminal:

```Shell
ssh <ip_address_here> -l deck
```

Type the root password of the Deck, and it you will be connected.