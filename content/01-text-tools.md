# Common Text Tools and Grepping

## head and tail

`head` and `tail` are great commands to show a specified number of lines. For example, if you want to display the last 5 lines from the file `/etc/passwd`, use the command syntax below:

- `tail -5 /etc/passwd`

`head` will do the same job but instead, it will display the first 5 lines from the file `/etc/shadow`:

- `head -5 /etc/shadow`

## cat

Another useful command to display the content of a file. This command is more useful when used with `less` or `more`:

- `cat /etc/passwd | more` — display the content of `/etc/passwd`
- `tac /etc/passwd | less` — same thing but this time from tail to head

## cut, sort, and tr

These 3 commands make a great utility. The `cut` command is used to cut lines using a delimiter, followed by `sort` which is used to sort a file in a precise format, while `tr` is used for translation. Let's learn how to use these commands. The following will display the first field from `/etc/passwd` in upper case and sort it in reverse:

```bash
cut -f1 -d: /etc/passwd | sort -r | tr [:lower:] [:upper:]
```

Below you will find a summary of the most common options for `cut` and `sort`:

### cut options

- `-b` — bytes
- `-c` — character
- `-f` — field
- `-d` — delimiter

### sort options

- `-r` — reverse
- `-d` — dictionary order
- `-n` — numerical value
- `-R` — random
- `-b` — ignore blanks
- `-o` — specify output file

**Note:** Another great command you can use with `sort` is `uniq`. Consult `man uniq` for more information.

## sed and awk

`sed` and `awk` are both powerful commands. `sed` (stream editor) is used for filtering and transforming text. Let me explain using some examples:

- `sed -i 's/old_text/new_text/g' <filename>` — this command will change the old_text to new_text in `<filename>`. The `-i` option is used to ensure the changes are saved in the file, `s` refers to substitute, and `g` refers to global.
- `sed -i '1d' <filename>` — deleting the first line from `<filename>`

`awk` is a very advanced command. It is a pattern scanning and processing language. Let me make it simple for you by using this command:

- `gawk -F: '{ print $1 }' /etc/passwd` — print the first field from `/etc/passwd`. The `-F` option refers to field separator.

## grep

`grep` is the most common command in Unix. It is used for filtering while looking deeply in file text. Below you will find a summary of the most common options for `grep`:

- `-R` — recursive
- `-v` — not containing "text"
- `-i` — ignore case
- `-f` — specify file
- `-l` — list the containing file names
- `-L` — list the file names without match
- `-A <n>` — print n lines after
- `-B <n>` — print n lines before
- `-C <n>` — print n lines before and after

**Note:** This does not cover all the text tools. Consult redhat.com for more information.
