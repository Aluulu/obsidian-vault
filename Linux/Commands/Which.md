Which is a [[Linux]] command that shows whenever a command is installed, and where the path to said command is. 

# Syntax

```Shell
which [command]
```

## Example

### Show the path for man

```Shell
which man
```

# Arguments

## --all, -a  
Print all matching executables in **PATH**, not just the first
  
## --read-alias, -i  
Read aliases from stdin, reporting matching ones on stdout. This is useful in combination with using an alias for which itself. For example  
```Shell
alias which=´alias | which -i´
```

## --skip-alias  
Ignore option `--read-alias`, if any. This is useful to explicity search for normal binaries, while using the `--read-alias` option in an alias or function for which
  
## --read-functions  
Read shell function definitions from stdin, reporting matching ones on stdout. This is useful in combination with using a shell function for which itself.  For example:  
```Shell
which() { declare -f | which --read-functions $@ }  
export -f which
```
  
## --skip-functions  
Ignore option `--read-functions`, if any. This is useful to explicity search for normal binaries, while using the `--read-functions` option in an alias or function for which.
  
## --skip-dot  
Skip directories in **PATH** that start with a dot.
  
## --skip-tilde  
Skip directories in **PATH** that start with a tilde and executables which reside in the HOME directory
  
## --show-dot  
If a directory in **PATH** starts with a dot and a matching executable was found for that path, then print `./programname` rather than the full path
  
## --tty-only  
Stop processing options on the right if not on tty
  
## --version,-v,-V  
Print version information on standard output then exit successfully
  
## --help  
Print usage information on standard output then exit successfully