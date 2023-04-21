Whereis is a [[Linux]] command which will locate the binary, source, and man pages for a command.

# Syntax

```Shell
whereis [command]
```

## Examples

### Find the binary, source, and man pages for pacman
```Shell
whereis pacman
```

### Find only the man pages for ping

```Shell
whereis -m ping
```

# Arguments

## -b
Search for binaries

## -m
Search for manuals

## -s
Search for sources

## -u
 Only show the command names that have unusual entries. A command is said to be unusual if it does not have just one entry of each explicitly requested type.
 Thus `whereis -m -u` asks for those files in the current directory which have no documentation file, or more than one

## -B list
Limit the places where `whereis` searches for binaries, by a whitespace-separated list of directories

## -M list
Limit the places where `whereis` searches for manuals and documentation in Info format, by a whitespace-separated list of directories

## -S list
Limit the places where `whereis` searches for sources, by a whitespace-separated list of directories

## -f
Terminates the directory list and signals the start of filenames. It must be used when any of the `-B`, `-M`, or `-S` options is used

## -l
Output the list of effective lookup paths that `whereis` is using. When none of `-B`, `-M`, or `-S` is specified, the option will output the hard-coded paths that the command was able to find on the system

## -h, --help
Display help text and exit

## -V, --version
Print version and exit
