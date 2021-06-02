# Sed Study Pt. 2

## The `-n` flag
- Sed works like a `cat` outputting the result to the terminal.
- Use the `-n` flag to supress the output.

## Feeding lines
- Usage: `sed 'l1,l2s/pattern/replacement/'`
```
$ sed '2,3s/are/be/ input'
Roses are red.
Violets be blue.
Let's see what else
vim and sed do.
```

## Printing only changed lines
- Usage: `sed -n 's/pattern/replacement/p'`
- `NOTE:` The above command with `///p` operates specially with `s///`
```
$ sed -n 's/are/be/p' input        
Roses be red.
Violets be blue.
```

## Expressions
- Usage: `$sed -e 'cmd1' -e 'cmd2`
- If no `-e` is passed, sed use the first non-argument option as the script to interpret.
- The below commands produce the same result:
```
$ sed 'cmd1; cmd2'
$ sed -e 'cmd1; cmd2'
$ sed -e 'cmd1' -e 'cmd2'
```

## Ranges
- Usage: `$sed 'begin,end/pattern/replacement/'`
```
$ sed '2,3s/are/were/' input 
Roses are red.
Violets were blue.
Let's see what else
vim and sed do.
```
- We can feed a search pattern to use it as begin/end also. It will use the first pattern as the begin range and the pattern after the comma as the end range.
- Usage: `$sed '/beginpattern/,/endpattern/s/are/be/'`
```
$ sed '/red/,/else/s/are/be/' input 
Roses be red.
Violets be blue.
Let's see what else
vim and sed do.
```

## Using braces (brace expansion)
- Braces can be used to group commands. It allow multiple commands to be operated on the range given.
- Used with range, like `$sed -n '2,3 {s/are/be/; p}'` it works like a if/else statement, where the program evaluates if the current line is between the range, and if it is, the pattern is applied.
- This tells sed to apply the command group on lines `l1` and `l2`
- Usage: `sed -n 'l1,l2 {s/pattern/replacement/; p}'`

## Printing the line number
- Usage: `$ sed '=' input`
```
$ sed '=' input                    
1
Roses are red.
2
Violets are blue.
3
Let's see what else
4
vim and sed do.
```

- We can combine it with the `-n` flag to output only the line numbers

```
$ sed -n '=' input 
1
2
3
4
```

- We can also combine with the `$` delimiter to print the total number of lines in the file ($ delimits the last line)
```
$ sed -n '$=' input
4
```

## 