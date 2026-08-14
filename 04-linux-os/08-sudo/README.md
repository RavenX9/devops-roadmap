# Sudo

`sudo` lets an authorized normal user run commands that require root-level privileges. This lesson covers switching to the root shell, safely editing sudo configuration, adding permissions, and switching between users.

> **Important:** Administrative access is powerful. Use the smallest permission set that meets the requirement, and validate every sudo configuration change with `visudo`.

## Switch to the Root User with `sudo -i`

A user with the required sudo privileges can start a root login shell with:

```bash
sudo -i
```

This changes the session from the current normal user to the root user. Depending on the sudo rule, the system may request the current user's password.

| Command | Description |
| --- | --- |
| `sudo -i` | Start a root login shell from an authorized user account. |

---

## Safely Edit Sudo Rules with `visudo`

Use `visudo` to open `/etc/sudoers` safely:

```bash
visudo
```

`visudo` validates the file's syntax before saving. Editing `/etc/sudoers` directly is risky: a syntax error can prevent users from using `sudo` system-wide.

Within the editor, you can search for the root rule and enable line numbers:

```vim
/root
:set nu
```

| Command | Description |
| --- | --- |
| `visudo` | Safely open the main sudoers file and check its syntax before saving. |
| `/root` | Search for the root user's rule in the editor. |
| `:set nu` | Show line numbers in the editor. |

> **Note:** The source lesson included terminal screenshots. They are intentionally omitted from this draft; use the commands above to reproduce the workflow without exposing machine-specific details.

---

## Add a User to the Sudoers File

To grant a user sudo access, edit the sudo configuration with `visudo` and add a rule modeled on the required administrative permissions. A standard sudo rule can prompt the user for their password before allowing `sudo -i`.

For automation or background scripts, the source describes the `NOPASSWD:` option, which suppresses that password prompt. Treat this option carefully because it grants elevated commands without interactive verification.

| Rule behavior | Result |
| --- | --- |
| Standard sudo rule | The authorized user is asked for a password when running privileged commands. |
| Rule containing `NOPASSWD:` | The authorized user can run the permitted privileged commands without a password prompt. |

---

## If the Sudoers File Has a Syntax Error

When you save through `visudo`, it checks the configuration first. If it finds a problem, it can show an error similar to:

```text
>>> /etc/sudoers: syntax error near line <LINE_NUMBER> <<<
```

It then provides options such as:

```text
(e)dit again
(x)exit without saving
(Q)uit and save bad file
```

Choose **`e`** to return to the editor and correct the syntax error. This validation is why `visudo` is the safer way to edit sudo configuration.

---

## Best Practice: Use `/etc/sudoers.d/`

Instead of placing custom rules directly in `/etc/sudoers`, keep them in a separate file under `/etc/sudoers.d/`. This keeps custom permissions separate from the main configuration file and lets `visudo` validate the custom file.

### 1. Go to the Include Directory

```bash
cd /etc/sudoers.d/
ls
```

This directory contains additional sudo configuration files.

### 2. Create a Custom Configuration File

```bash
touch <CONFIG_FILE>
```

Create a separate file for the custom permissions.

### 3. Edit It with `visudo`

```bash
visudo -f /etc/sudoers.d/<CONFIG_FILE>
```

The `-f` option tells `visudo` to open and validate the specified file.

### 4. Add Group Permissions

The source lesson uses this example rule:

```text
%devops ALL=(ALL) NOPASSWD: ALL
```

Read it as: members of the `devops` group can run any command as any user without being prompted for a password.

> **Note:** The `%` prefix identifies a group rather than an individual user. Because this rule grants unrestricted passwordless access, review it carefully before using it.

| Command or rule | Description |
| --- | --- |
| `cd /etc/sudoers.d/` | Move to the directory for additional sudo configuration files. |
| `ls` | List the existing files in the current directory. |
| `touch <CONFIG_FILE>` | Create a custom sudo configuration file. |
| `visudo -f /etc/sudoers.d/<CONFIG_FILE>` | Safely edit and validate the specified custom configuration file. |
| `%devops ALL=(ALL) NOPASSWD: ALL` | Example group rule granting unrestricted passwordless sudo access. |

---

## Switch Between User Accounts with `su -`

Use `su - <USERNAME>` to switch from the current user to another user:

```bash
su - <USERNAME>
```

A normal user usually needs the target user's password to switch accounts. The root user can switch to another user without entering that user's password.

| Command | Description |
| --- | --- |
| `su - <USERNAME>` | Start a login shell as the specified user. |
