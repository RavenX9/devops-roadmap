# Filters & Redirection

Linux filters help you search, inspect, and transform text. Redirection and pipes control where command input and output go. Together, these tools make it easier to work with files and command output from the terminal.

---

## `grep`

`grep` searches text input for matching patterns. Linux is case-sensitive, so uppercase and lowercase letters are different unless you use an option such as `-i`.

### Syntax

```bash
grep <OPTION> <SEARCH_TERM> <FILE_PATH>
```

A command such as the following reads its input from the file:

```bash
grep -i firewall <FILE_PATH>
```

The file is effectively supplied as standard input, even when the `<` symbol is not written explicitly:

```bash
grep -i firewall < <FILE_PATH>
```

### Common `grep` examples

| Command | Description |
| --- | --- |
| `grep -i <SEARCH_TERM> <FILE_PATH>` | Search while ignoring letter case. |
| `grep firewall *` | Search files in the current directory for `firewall`. |
| `grep -R firewall *` | Recursively search from the current directory for `firewall`. |
| `grep -R SELINUX /etc/*` | Recursively search under `/etc` for `SELINUX`. |
| `grep -vi firewall <FILE_PATH>` | Display lines that **do not** contain `firewall`, ignoring case. |
| `grep -E "PATTERN_ONE\|PATTERN_TWO" <FILE_PATH>` | Search for either pattern with extended regular expressions. |
| `grep -B 2 -A 2 'pattern' <FILE_PATH>` | Show two lines before and two lines after each match. |

`-E` enables extended regular expressions. Inside a quoted expression, `|` means “or”; quoting prevents the shell from treating `|` as a pipe.

> **Sensitive-data note:** Do not use `grep` to expose real secrets. If you need to demonstrate matching configuration keys, use placeholders such as `DB_NAME`, `DB_USER`, and `DB_PASSWORD`; never paste credentials into notes or terminal output.

---

## Viewing files with `less` and `more`

### `less`

Use `less` to read a file one screen at a time without opening an editor.

```bash
less <FILE_PATH>
```

Think of it as: “Open this file so I can scroll and read it.”

| Key | Action |
| --- | --- |
| `↑` / `↓` | Move one line at a time. |
| `Space` | Move to the next page. |
| `b` | Move to the previous page. |
| `Page Down` | Scroll down. |
| `Page Up` | Scroll up. |
| `/searchterm` | Search for text. |
| `n` | Go to the next search match. |
| `N` | Go to the previous search match. |
| `q` | Quit. |

### `more`

`more` also displays a file page by page, but has fewer features than `less`.

```bash
more <FILE_PATH>
```

| Key | Action |
| --- | --- |
| `Space` | Move to the next page. |
| `Enter` | Move to the next line. |
| `q` | Quit. |

---

## `head` and `tail`

### `head`

`head` prints the first 10 lines of a file by default.

```bash
head <FILE_PATH>
head -20 <FILE_PATH>
```

The second command prints the first 20 lines.

### `tail`

`tail` prints the last 10 lines of a file by default.

```bash
tail <FILE_PATH>
tail -20 <FILE_PATH>
```

The second command prints the last 20 lines.

Use `-f` to watch a file as new content is added. This is useful when monitoring log files.

```bash
tail -f <LOG_FILE>
```

---

## `cut`

`cut` filters text by selecting fields. It is commonly used with:

- `-d` — the **delimiter**, the character that separates fields.
- `-f` — the field number to display.

### Common delimiters

| Delimiter | Meaning |
| --- | --- |
| `:` | Colon |
| `" "` | Space |
| `,` | Comma |
| `\t` | Tab (the default delimiter when `-d` is not specified) |

### Example: list local account names

`/etc/passwd` uses `:` as its separator. The first field is the account name.

```bash
cut -d ':' -f 1 /etc/passwd
```

You may also write the delimiter without spaces:

```bash
cut -d: -f 1 /etc/passwd
```

### When quotes matter

Quotes matter when the delimiter is a space or another shell-special character.

```bash
cut -d ' ' -f 1 <FILE_PATH>
```

Without quotes, the shell treats the space as an argument separator and the command will not receive the intended delimiter.

---

## `awk`

`awk` is a more advanced filtering tool. It processes a file line by line and can take an action when a pattern matches.

> “For each line in a file, if a pattern matches, do an action.”

### Syntax

```bash
awk 'pattern { action }' <FILE_PATH>
```

By default, `awk` uses whitespace (spaces and tabs) as its field separator. Use `-F` when your file uses a different separator.

```bash
awk -F: '{print $1}' /etc/passwd
```

Read this as: “Use `:` as the field separator, then print field 1 for each line in `/etc/passwd`.” This is equivalent in purpose to using `cut -d ':' -f 1`.

### Useful `awk` variables

| Variable | Meaning |
| --- | --- |
| `$1` | First field |
| `$2` | Second field |
| `$3` | Third field |
| `$0` | Entire line |
| `NF` | Number of fields |

### Example 1: print the first field

Given a file containing:

```text
user-one 25 developer
user-two 30 designer
```

Run:

```bash
awk '{print $1}' <FILE_PATH>
```

Output:

```text
user-one
user-two
```

### Example 2: print selected fields

```bash
awk '{print $1, $3}' <FILE_PATH>
```

Output:

```text
user-one developer
user-two designer
```

---

## `sed` — search and replace

`sed` lets you search and substitute text without manually editing the file.

### Preview a replacement

```bash
sed 's/coronavirus/covid19/gi' <FILE_PATH>
```

| Part | Meaning |
| --- | --- |
| `s` | Substitute. |
| `g` | Replace all matches on each line. |
| `i` | Ignore case. |
| `<FILE_PATH>` | Input file. |

This command **does not modify the file**. It prints the modified result to the terminal while leaving the original file unchanged.

> Think of it as: “Show me what the file would look like after the changes.”

### Edit a file in place

```bash
sed -i 's/coronavirus/covid19/gi' <FILE_PATH>
```

`-i` edits the file directly.

A safer habit is to create a backup at the same time:

```bash
sed -i.bak 's/coronavirus/covid19/gi' <FILE_PATH>
```

This modifies the target file and creates a backup named `<FILE_PATH>.bak`.

### Substitute text in Vim

Open the file in Vim, enter command-line mode with `:`, then run:

```vim
:%s/coronavirus/covid19/gi
```

| Part | Meaning |
| --- | --- |
| `%` | Entire file. |
| `s` | Substitute. |
| `coronavirus` | Search text. |
| `covid19` | Replacement text. |
| `g` | Replace all occurrences on each line. |
| `i` | Ignore case. |

For help in Vim:

```vim
:h :s
:help :s
:help s_flags
:h substitute
```

---

# Redirection

Redirection sends command output to a file. The two common output-redirection operators are `>` and `>>`.

## `>` — overwrite a file

`>` redirects output to a file and **replaces** everything that was already in it.

```bash
echo "hello" > file.txt
```

If `file.txt` originally contains:

```text
apple
banana
```

after the command it contains:

```text
hello
```

The old content is removed.

## `>>` — append to a file

`>>` redirects output and adds new content to the **end** of a file while keeping existing content.

```bash
echo "hello" >> file.txt
```

If `file.txt` originally contains:

```text
apple
banana
```

after the command it contains:

```text
apple
banana
hello
```

---

## `wc`

`wc` counts content in a file. Use `-l` to count lines.

```bash
wc -l /etc/passwd
```

This reports the number of lines in `/etc/passwd`.

---

## Pipes: `|`

A pipe sends the output from the program on the left to the input of the program on the right.

```bash
command1 | command2
```

Read it as: “Run `command1`, then send its output as input to `command2`.”

### Example: count items listed by `ls`

```bash
ls | wc -l
```

| Part | Meaning |
| --- | --- |
| `ls` | Lists directory contents. |
| `|` | Sends the command on the left to the command on the right. |
| `wc -l` | Counts received lines. |

If `ls` outputs:

```text
file1
file2
file3
```

then `wc -l` receives those three lines and outputs:

```text
3
```

### More pipe examples

```bash
ls | grep host
tail -20 /var/log/messages | grep -i vagrant
```

The first command lists the current directory and shows entries containing `host`. The second command displays the last 20 lines of `/var/log/messages` and searches them for `vagrant`, ignoring case.

---

## `find`

`find` searches for files or directories by path and conditions.

### Syntax

```bash
find <STARTING_DIRECTORY> <CONDITIONS>
```

### Examples

```bash
find /etc -name hosts
find . -iname "readme.txt"
find . -name "*.txt"
```

| Command | Meaning |
| --- | --- |
| `find /etc -name hosts` | Starting in `/etc`, find files or directories named `hosts`. |
| `find . -iname "readme.txt"` | Starting in the current directory, find `readme.txt` while ignoring letter case. |
| `find . -name "*.txt"` | Starting in the current directory, find files or directories whose names end in `.txt`. |
