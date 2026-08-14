# Ubuntu Commands

## Ubuntu vs. CentOS: Key Differences

Most commands learned on CentOS also work on Ubuntu. This guide focuses on the important differences in user management, package management, and common tools.

---

## User Management

On CentOS, `useradd` creates a user with a home directory and mail spool automatically. On Ubuntu, `useradd` does not create a home directory or mail spool.

A simpler Ubuntu option is `adduser`:

```bash
adduser <USERNAME>
```

`adduser` creates the user and its group, adds the user to that group, creates the home directory, copies the default files from `/etc/skel`, and prompts you to set a password.

### `visudo` Default Editor

On CentOS, `visudo` opens in VIM. On Ubuntu, the default editor is Nano. To temporarily use VIM instead, run:

```bash
export EDITOR=vim
```

Then run `visudo`; it will open in VIM. This setting is lost when you log out. The source notes that making it permanent is covered later in Bash scripting.

---

## Package Management

Ubuntu uses `apt` where CentOS uses `yum`. Ubuntu's primary repository configuration file is `/etc/apt/sources.list`, and additional repository files are stored in `/etc/apt/sources.list.d/`.

> **Important:** Run `apt update` before installing software to refresh the package list from repositories. With `yum`, this refresh happens automatically.

| Command | Description |
| --- | --- |
| `apt update` | Refresh the package list from repositories. |
| `apt search tree` | Search for a package. |
| `apt install apache2 -y` | Install a package with its dependencies. |
| `apt remove apache2 -y` | Remove a package while keeping its configuration and data. |
| `apt purge apache2` | Remove a package together with all configuration and data. |
| `apt upgrade` | Upgrade all installed packages. |

> **Note:** When Ubuntu installs a service such as `apache2`, it starts and enables it automatically. On CentOS, you must do this manually with `systemctl start` and `systemctl enable`.

### Manual Package Installation with `dpkg`

`dpkg` is Ubuntu's equivalent of `rpm`. It installs `.deb` packages directly without resolving dependencies.

```bash
wget <PACKAGE_URL> -O tree.deb
dpkg -i tree.deb
```

| Command | Description |
| --- | --- |
| `dpkg -i tree.deb` | Install a `.deb` package. |
| `dpkg -l` | List all installed packages. |
| `dpkg -l \| grep tree` | List installed packages, pass the output to `grep tree`, and search for lines containing `tree`. |
| `dpkg -r tree` | Remove the `tree` package. |

---

## Quick Comparison

| Feature | CentOS | Ubuntu |
| --- | --- | --- |
| User creation | `useradd` | `adduser` |
| Package manager | `yum` / `dnf` | `apt` |
| Manual installation | `rpm` | `dpkg` |
| Package format | `.rpm` | `.deb` |
| Repository configuration | `/etc/yum.repos.d/` | `/etc/apt/sources.list` |
| Service automatically starts on installation | No | Yes |
| Firewall | `firewalld` | `ufw` |
