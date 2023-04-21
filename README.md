# obsidian-vault

A collection of my obsidian vault holding general and specific knowledge of IT

# Plugins

This vault uses the [Advanced tables](https://github.com/tgrosinger/advanced-tables-obsidian) add-on for easier table use. Download it by clicking the cog in the bottom left > Community plugins > Enable Community plugins > Browse > Advanced Tables > Download and enable.

# Notations and typographical conventions

## Placeholders

Placeholder code (arguments that should not be taken literally) are in square brackets, for example:

```Shell
cp [file] [directory]
```

## Repeated commands

Commands with an ellipsis `(...)` can be repeated, for example:

```Shell
pacman -S [package_name] [...]
```

Pacman can install multiple packages at once when specified, this can be done by typing the name of the package after selecting the install flag.

## Selection

Curly braces with a vertical bar indicate a selection between multiple choices, for example:

```Shell
pacman { -S | -R | -Rs } [package_name]
```

## Wildcard

A star `( * )` indicates one or more characters. or a selection for everything. For example:

```Shell
# The star indicates that it is a palceholder for the middle part of this command
pacman -R visual-*-code
```

```Shell
# This indicates replacing the * with whatever you want, in this case the packages to uninstall
Pacman -R *
```

## Tilde

The tilde `( ~ )` means the home directory of the user. For example:

```Shell
cp file.txt ~/documents/backup/
```