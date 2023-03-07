Grep is a [[Linux]] command that finds patterns in files using regular expressions.

## Syntax

### Search for a pattern within a file:

```Shell
grep "<search_pattern>" path/to/file
```

### Search for an exact string (disables regular expressions):

```Shell
grep --fixed-strings "<exact_string>" path/to/file
```

### Search for a pattern in all files recursively in a directory, showing line numbers of matches, ignoring binary files:

```Shell
grep --recursive --line-number --binary-files=without-match "<search_pattern>" path/to/directory
```
### Use extended regular expressions (supports `?`, `+`, `{}`, `()` and `|`), in case-insensitive mode:

```Shell
grep --extended-regexp --ignore-case <search_pattern>" path/to/file
```

### Print 3 lines of context around, before, or after each match:

```Shell
grep --context|before-context|after-context=3 "search_pattern" path/to/file
```

### Print file name and line number for each match with color output:

```Shell
grep --with-filename --line-number --color=always "search_pattern" path/to/file
```

### Search for lines matching a pattern, printing only the matched text:

```Shell
grep --only-matching "search_pattern" path/to/file
```

### Search `stdin` for lines that do not match a pattern:

```Shell
cat path/to/file | grep --invert-match "search_pattern"
```


## Examples

### Search for the word "Hello" in file

``` Shell
grep "Hello" path/to/file
```

#### Search for the world "Hello" in multiple files

```Shell
grep "Hello" /path/to/file /path/to/file2 /path/to/file3
```

#### Search for case-insensitive "Hello" in file

```Shell
grep -i "Hello" path/to/file
```

#### Search only for "Hello" as an exact statement in file

```Shell
grep -w "Hello" path/to/file
```

#### Count how many lines include "Hello" in a file

```Shell
grep -c "Hello" path/to/file
```

#### Search for lines that DO NOT have "Hello" in a file

```Shell
grep -v "Hello" path/to/file
```

#### Print out the line number where "Hello" is present

```Shell
grep -n "Hello" path/to/file
```

#### Print out only "Hello" in the file

```Shell
grep -o "Hello" path/to/file
```


## References

Syntax - https://github.com/tldr-pages/tldr/blob/main/pages/common/grep.md
Examples - https://www.golinuxcloud.com/grep-command-in-linux/