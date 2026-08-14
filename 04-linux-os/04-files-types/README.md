# Files Types

In Linux, the first character shown by a long file listing identifies the file type. Learning to read it helps you distinguish ordinary files from directories, links, device files, and inter-process communication files.

---

## Identify file types with `ls -l`

Run the following command to view a long listing:

```bash
ls -l
```

Example output:

```text
-rw-r--r--  file.txt
drwxr-xr-x  myfolder
lrwxrwxrwx  mylink -> /tmp/file
crw-rw-rw-  /dev/null
srwxr-xr-x  mysocket
prw-r--r--  mypipe
brw-rw----. root disk sda
```

The **first character** of each entry describes its type:

| Character | File type | Description |
|---|---|---|
| `-` | Regular file | A normal file, such as a text file, configuration file, script, data file, or executable. |
| `d` | Directory | A file that contains a list of other files and directories. |
| `l` | Symbolic link | A shortcut or reference that points to another file or directory. |
| `c` | Character device file | A special input/output file that processes data one character at a time. |
| `s` | Socket file | A special file used for inter-process communication (IPC) and networking between processes. |
| `p` | Pipe (named pipe / FIFO) | A special file that lets one process send data to another. |
| `b` | Block device file | A special device file represented as a block device. |

To confirm the type of a specific item, use `file`:

```bash
file <FILE_PATH>
```

For example:

```bash
file yum
```

---

## `ls` command options

| Command | Description |
|---|---|
| `ls -l` | Lists files and directories in long format, one item per line. It shows details such as permissions, owner, size, and date. |
| `ls -a` | Lists all files and directories, including hidden entries whose names start with `.` such as `.git`, `.bashrc`, `.vimrc`, and `.ssh`. |
| `ls -F` | Adds a classification symbol to each name; `/` marks a directory and `*` marks an executable file. |
| `ls -g` | Lists files and directories with group-name information. |
| `ls -i` | Shows the inode (index) number for files and directories. |
| `ls -m` | Lists files and directories separated by commas. |
| `ls -n` | Uses numeric UID and GID values instead of owner and group names. |
| `ls -r` | Lists files and directories in reverse order. |
| `ls -R` | Lists files and directories recursively, including subdirectories. |
| `ls -t` | Sorts files by modification time, with the newest first. |

### Common combinations

```bash
# List all files, including hidden files, in long format
ls -la

# List in long format, sort by modification time, then reverse the order
# (oldest first)
ls -ltr

# Recursively list the contents of /home and its subdirectories
ls -R /home
```

---

## Create a symbolic link with `ln -s`

A symbolic link (or soft link) is a shortcut that points to another file or directory.

### Syntax

```bash
ln -s <TARGET_FILE> <SYMBOLIC_LINK_NAME>
```

This creates a shortcut named `<SYMBOLIC_LINK_NAME>` that points to `<TARGET_FILE>`.

### Example

```bash
ln -s <TARGET_FILE_PATH> cmds
```

This creates a symbolic link named `cmds` that points to `<TARGET_FILE_PATH>`.

> If the target file is moved or deleted, the symbolic link becomes broken. In many terminal themes, a broken link is highlighted in red.
