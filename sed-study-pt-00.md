# How sed works

```
<What happens in Memory>

█   Input   █ Pattern   █ Output    █
█           █ Space     █           █
█           █           █           █
█           █           █           █
█           █           █           █
█           █           █           █
█           █           █           █

```

## A line reader
- Sed works basically with three elements: the input, the pattern space and the output and reads a file line by line.

## The input
- The input corresponds to the file or chunk of text you want to feed to sed.

- It can be piped to sed or passed in the same command:
```
$ cat input | sed ''
```
```
$ <input > sed ''
```
```
$ sed '' input
```

## The pattern space
- Is the area in memory where sed operates all of its commands.

- In the case `$ sed '' input` sed gets the first line, puts it on the pattern space, then operates the commands inside `''`, passes the line to the output and proceeds to the next line.

- When we use the `$sed -n '' input` flag, it supresses the output after putting the line on the pattern space.

- In other case, when we say `$sed -n 'p' input` it prints whatever is in the pattern space to the output. Then it deletes whatever is in the pattern space and moves to the next line.
```
$ sed -n 'p' input 
Roses are red.
Violets are blue.
Let's see what else
vim and sed do.
```

- If we supply only `$sed 'p' input` without the `-n` flag it will copy the input line to the pattern space, explicitly copy the line in the pattern space to the output then it will operate the default behavior of copying the line in the pattern space and putting it into the output. This will double the lines in the output.
```
$ sed 'p' input   
Roses are red.
Roses are red.
Violets are blue.
Violets are blue.
Let's see what else
Let's see what else
vim and sed do.
vim and sed do.
```

- We can use `$sed 'd' input` to delete whatever is in the pattern space and move it to the next line. In that case, it won't print anything by default, since we are deleting whatever is in the pattern space causing sed to not have anything in the pattern space to pass it over to the output.

## The output
- It is what will be, by default, printed to the screen after sed running its commands in the pattern space.