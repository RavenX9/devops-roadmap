# Package Management

Package management is typically an administrative task, so run installation and removal commands with the privileges required by your system.

---

## Understanding packages

When a command is unavailable, the related software package may not be installed. Packages are stored in repositories, and a package must match the operating system and CPU architecture.

### Check the system architecture

Use either command to view the architecture:

```bash
arch
# or
uname -m
```

For example, `x86_64` means a 64-bit x86 architecture:

- **x86** refers to the Intel 8086 processor family and its descendants.
- **64** indicates a 64-bit architecture.

### Download and install an RPM package manually

After locating a package that matches the system architecture, copy its download URL and download it into the virtual machine:

```bash
curl <PACKAGE_URL> -o <PACKAGE_FILE>.rpm
# or
wget <PACKAGE_URL> -O <PACKAGE_FILE>.rpm
```

The `-o` option in the `curl` example saves the download with the filename you specify. Confirm that the file was downloaded:

```bash
ls
```

Install the package file directly with RPM:

```bash
rpm -ivh <PACKAGE_FILE>.rpm
```

| Option | Meaning |
|---|---|
| `-i` | Install the package. |
| `-v` | Show verbose, detailed output. |
| `-h` | Show installation progress with hash (`#`) marks. |

Verify an installed package with either command:

```bash
rpm -qa | grep <PACKAGE_NAME>
# or
rpm -q <PACKAGE_NAME>
```

> **Important:** `rpm -ivh` installs an RPM file directly and does **not** resolve dependencies automatically. Installing through `yum` can resolve the required dependencies.

---

## Why package managers exist

A manual RPM installation can fail when the package needs software that is not already installed. For example, installing an HTTP server RPM may report missing dependencies such as `httpd-core` or `system-logos-httpd`.

A package can require tens or hundreds of dependencies. Locating, downloading, and installing each one manually is slow and error-prone. Package managers automate this work: they locate, download, install, and manage required dependencies.

---

## Package management tools

### RPM (Red Hat Package Manager)

RPM installs, removes, and queries RPM packages directly.

```bash
rpm -ivh <PACKAGE_FILE>.rpm
```

RPM does not automatically resolve dependencies.

### YUM (Yellowdog Updater Modified)

YUM is a package manager for RPM-based distributions such as CentOS and RHEL. It automatically resolves and installs dependencies.

```bash
yum install httpd -y
```

YUM repository configuration files are stored in:

```text
/etc/yum.repos.d/
```

List the configured repositories:

```bash
ls /etc/yum.repos.d/
```

Additional repositories can be added to access more software packages.

#### Useful YUM commands

| Command | Description |
|---|---|
| `yum --help` | Display available YUM commands and options. |
| `yum search httpd` | Search configured repositories for packages matching `httpd`. |
| `yum install httpd -y` | Install the HTTP server package and its required dependencies. |
| `yum update` | Update installed packages. |
| `yum remove httpd -y` | Remove the HTTP server package. |

### DNF (Dandified YUM)

DNF is the next-generation replacement for YUM on modern RPM-based distributions. It provides faster dependency resolution, improved performance, and improved package management.

| Command | Description |
|---|---|
| `dnf --help` | Display available DNF commands and options. |
| `dnf search httpd` | Search repositories for packages matching `httpd`. |
| `dnf install httpd -y` | Install the package and required dependencies. |
| `dnf remove httpd -y` | Remove the package and unused dependencies no longer required by other packages. |
| `dnf update` | Update installed packages. |

### APT (Advanced Package Tool)

APT is used by Debian- and Ubuntu-based distributions. Like YUM, it automatically resolves and installs dependencies.

APT repository locations are listed in:

```text
/etc/apt/sources.list
```

View the configured APT repositories:

```bash
cat /etc/apt/sources.list
```

#### Useful APT commands

| Command | Description |
|---|---|
| `apt --help` | Display available APT commands and options. |
| `apt update` | Refresh package lists from configured repositories. |
| `apt search apache2` | Search repositories for packages matching `apache2`. |
| `apt install apache2 -y` | Install Apache and its required dependencies. |
| `apt remove apache2 -y` | Remove Apache. |

### DPKG (Debian Package Manager)

DPKG installs and manages `.deb` package files directly.

```bash
wget <PACKAGE_URL> -O <PACKAGE_FILE>.deb
dpkg -i <PACKAGE_FILE>.deb
```

Like RPM, `dpkg` does not automatically resolve dependencies.

---

## Quick comparison

| Tool | Typical distribution family | Main use | Dependency handling |
|---|---|---|---|
| `rpm` | RPM-based systems | Install and query RPM files directly | Does not resolve dependencies automatically |
| `yum` | RPM-based systems | Install and manage repository packages | Resolves dependencies automatically |
| `dnf` | Modern RPM-based systems | Modern replacement for YUM | Resolves dependencies automatically |
| `apt` | Debian and Ubuntu systems | Install and manage repository packages | Resolves dependencies automatically |
| `dpkg` | Debian and Ubuntu systems | Install and manage `.deb` files directly | Does not resolve dependencies automatically |

### Easy way to remember

- `rpm` and `dpkg` install package files directly.
- `yum` and `apt` install packages and handle dependencies automatically.
- `dnf` is the modern replacement for `yum`.
