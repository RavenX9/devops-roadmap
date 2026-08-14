# VIM Editor - Visual Display Editor Improved

VIM is a text editor used to open and edit files. The `vi` editor already exists on many Linux systems, while VIM is an enhanced version.

## Install VIM

As the `vagrant` user:

```bash
sudo yum install vim -y
```

As the `root` user:

```bash
yum install vim -y
```

After installation, check the version and installation path:

```bash
vim --version
which vim
```

## Modes in VIM Editor

There are three modes in the VIM editor:

- **Command mode** — Use `Esc` to return to this mode.
- **Insert mode (edit mode)** — Press `i` to enter this mode and make changes.
- **Extended command mode** — Press `:` to enter this mode for saving and exiting.

> **Note:** When you open VIM, it starts in command mode.

---

### Command Mode

| Command | Description |
| --- | --- |
| `gg` | Go to the beginning of the page. |
| `G` | Go to the end of the page. |
| `w` | Move the cursor forward, word by word. |
| `b` | Move the cursor backward, word by word. |
| `nw` | Move the cursor forward by *n* words. Example: `5w`. |
| `nb` | Move the cursor backward by *n* words. Example: `5b`. |
| `u` | Undo the last change. |
| `U` | Undo all previous changes on the current line. |
| `Ctrl + R` | Redo undone changes. |
| `yy` | Copy (yank) the current line. |
| `nyy` | Copy *n* lines from the cursor position. Example: `5yy` or `4yy`. |
| `p` | Paste below the cursor position. |
| `P` | Paste above the cursor position. |
| `dw` | Delete a word forward, letter by letter (similar to Backspace behavior). |
| `x` | Delete one character at the cursor position (similar to the Delete key). |
| `dd` | Delete the entire current line. |
| `ndd` | Delete *n* lines from the cursor position. Example: `5dd`. |
| `/word` | Search for a word in the file. Example: `/hello`. |

---

### Extended Mode

Press `Esc` to return to command mode, then use `:` followed by an extended command.

| Command | Description |
| --- | --- |
| `:w` | Save changes to the file. |
| `:q` | Quit the editor after saving changes, or quit when there are no changes. |
| `:q!` | Force quit without saving changes. |
| `:wq` | Save changes and quit the editor. |
| `:w!` | Force save the file; useful when permissions prevent saving. |
| `:wq!` | Force save and quit. |
| `:x` | Save and quit (similar to `:wq`). |
| `:X` | Add or remove a password for the file. |
| `:20` or `:n` | Go to line 20 or another specified line number. |
| `:set nu` | Show line numbers in the file. |
| `:set nonu` | Hide line numbers in the file. |
