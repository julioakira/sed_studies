# Sed Study Pt. 1

### First Match

- To make a simple replacement on the first match of a single file:
```
$ cat text.txt
10 tiny toes
this is that
5 funny 0
one two three
tree twice
$ cat text.txt | sed 's/t/T/'
// Can also be done with
$ sed 's/t/T/' text.txt
10 Tiny toes
This is that
5 funny 0
one Two three
Tree twice
```

### Global Replacement

-`/g/` on the end of the command:
```
$ sed 's/t/T/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
```

### Replace and save on disk

- `-i` flag.
- `CAUTION:` There is no turning back.
```
// -i stands for inline?
$ sed -i 's/t/T/g' text.txt
$ cat text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
```

### Replace only in the beggining of a line
- `^pattern` on 's/^match_pattern/replacement_pattern/g'.

```
$ sed 's/^t/oo/g' text.txt
10 Tiny Toes
oohis is ThaT
5 funny 0
one Two Three
ooree Twice
```

### Replace only in the ending of a line
- `pattern$` on 's/match_pattern$/replacement_pattern/g'.
```
$ sed 's/t$/oo/g' text.txt
10 tiny toes
this is thaoo
5 funny 0
one two three
ooree twice
```

### Replace all numeric ocurrencies
- `[0-9]` on 's/match_pattern/replacement_pattern/g'
```
$ sed -i 's/t/T/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
$ sed 's/[0-9]/*/g' text.txt
** Tiny Toes
This is ThaT
* funny *
one Two Three
Tree Twice
```

### Replace all lowercase character ocurrencies
- `[a-z]` on 's/match_pattern/replacement_pattern/g'
```
$ sed -i 's/t/T/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
$ sed 's/[a-z]/*/g' text.txt
10 T*** T***
T*** is T**T
5 ***** 0
** T** T****
T*** T****
```

### Replace all lowercase interval based character ocurrencies
- `[a-h]` on 's/match_pattern/replacement_pattern/g' (for a to h)
- `[0-m]` like patterns also work halfway
```
$ sed -i 's/t/T/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
$ sed 's/[a-h]/*/g' text.txt
```

### Replace all uppercase character ocurrencies
- `[A-Z]` on 's/match_pattern/replacement_pattern/g'
```
$ sed -i 's/t/T/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
$ sed 's/[A-Z]/*/g' text.txt
10 *iny *oes
*his is *ha*
5 funny 0
one *wo *hree
*ree *wice
```

### Replace all lowercase and uppercase occurencies
- `[a-zA-Z]` on 's/match_pattern/replacement_pattern/g'
- `[A-z]` on 's/match_pattern/replacement_pattern/g'
- `NOTE:` second one works because in ASCii, capital letters come first. Won't work with `[a-Z]`
```
$ sed -i 's/t/T/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
$ sed 's/[A-z]/*/g' text.txt
10 *** ***
*****
5 ** 0
***
**** ******
```

### Replace all alphanumeric occurencies
- `[A-z0-9]` on 's/match_pattern/replacement_pattern/g'
- `[0-z]` on 's/match_pattern/replacement_pattern/g'
```
$ sed -i 's/t/T/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
$ sed 's/[A-z0-9]/*/g' text.txt
** *** ***
*****
* ** *
***
**** ******
```
### Replace all ocurrencies followed by another pattern
- `[pattern1]pattern2` like `[A-Z]i`
```
$ sed -i 's/t/T/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
$ sed 's/[A-Z]i/*/g' text.txt
10 *ny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
```

### Replace all occurencies with itself plus another pattern
- `[match_pattern]/(&)` ( ) can be any other character you want
- We can do `[match_pattern]/(&&)` to replace with two occurencies of the given pattern
- For double plus digits numbers, we can do `[match_pattern]\+/`
```
$ sed 's/[0-9]/(&)/g' text.txt
(1)(0) Tiny Toes
This is ThaT
(5) funny (0)
one Two Three
Tree Twice
// For double digits+ numbers
$ sed 's/[0-9]\+/(&)/g' text.txt
(10) Tiny Toes
This is ThaT
(5) funny (0)
one Two Three
Tree Twice
```

### Defining a custom delimiter
- We can define a custom delimiter we want with
```
// Using _ as delimiter
$ sed 's_[0-9]_**_g' text.txt
**** Tiny Toes
This is ThaT
** funny **
one Two Three
Tree Twice
```

## Pattern negation inside brackets pattern
- `[^pattern1]/replacement` with `^` inside brackets
```
$ sed 's/[^0-9]/*/g' text.txt
10**********
************
5*******0
*************
**********
*****************
```

## Matching whitespaces
- Match with `/[[:space:]]/replacement/g`
- `NOTE:` I don't know if this applies to any UNIX version
```
$ sed 's/[[:space:]]/*/g' text.txt
10*Tiny*Toes
This*is*ThaT
5*funny*0
one*Two*Three
Tree*Twice
/usr/share/local/
```

## Matching tabs
- Match with `/[\t]/replacement/g`
- `NOTE:` I don't know if this applies to any UNIX version
```
$ sed 's/[\t]/*/g' text.txt
// More fancy method
$ TAB=$(printf '\t')
$ sed "s/${TAB}/*/g" text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
Tree Twice
/usr/share/local/
*this is a tab
```

## Replacing more than one string
- Separate commands with a semicolon `;`
```
$ sed 's/Twice/None/g;s/Two/Three/g' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Three Three
Tree None
/usr/share/local/
        this is a tab
```

## Removing or replacing the first or last word of each line
- Usage: `'s/\w*.//'`
- `NOTE:` To replace in the last line, use `'s/\w*$//g'`
- `\w` word
- `*` followed by any other character
- `.` any character except new line (\n)
```
$ sed 's/\w*//' text.txt     
 Tiny Toes
 is ThaT
 funny 0
 Two Three
 Twice
/usr/share/local/
        this is a tab
```

- Can also be done with `s/^\w\+ \+//g`
- `^` starts from the left
- `\w` word
- `+` 1 or more
- `(space)` matches a space
```
$ sed 's/^\w\+ \+//g' text.txt
Tiny Toes
is ThaT
funny 0
Two Three
Twice
/usr/share/local/
        this is a tab
```

## Display only lines with matches
- Usage: `$sed -n 's/pattern/substitute/p'`
- Idea: don't show anything, except for the matches.
- `-n` silent mode
- `p` print matches
```
$ sed -n 's/T/t/p' text.txt 
10 tiny Toes
this is ThaT
one two Three
tree Twice
```

## Ignore case
- Usage: `'s/pattern/replacement/Ig'`
- `I` ignores case
```
$ sed 's/that/hi/Ig' text.txt
10 Tiny Toes
This is hi
5 funny 0
one Two Three
Tree Twice
/usr/share/local/
        this is a tab
```
## Using script files
- Usage: `sed -f sedscriptfile targetfile` 
- `f` run from a given file
```
$ sed -f sedscriptfile text.txt 
tiny toEs
is that
funny 0
two thrEE
twicE
usr/sharE/local/
this is a tab
```

## Word boundaries
- Usage: `'s/\bpattern\b/replacement/g'`
- `\b` word boundary
```
$ sed 's/\bthis\b/lalala/gI' text.txt
10 Tiny Toes
lalala is ThaT
5 funny 0
one Two Three
Tree Twice
/usr/share/local/
        lalala is a tab
```

## Remove lines with match
- Usage: `'/pattern/d'`
- `d` deletes the line with match
- `NOTE:` to ignore case use `'/pattern/Id'`
```
sed '/Tiny/d' text.txt             
This is ThaT
5 funny 0
one Two Three
Tree Twice
/usr/share/local/
        this is a tab
```

# Display X number of lines
- Usage: `$sed 'X q'`
- `X` line number
- `q` quit after doing the command before
```
$ sed '4 q' text.txt
10 Tiny Toes
This is ThaT
5 funny 0
one Two Three
```
