# File Permissions

Linux permissions control who can read, change, or run a file, and who can access a directory. This guide explains how to view permissions, change ownership, and update permissions with symbolic or numeric modes.

## View Permissions from the Command Line

Use `ls -l` to view a file in long-listing format. Use `ls -ld` to view the permissions of a directory itself rather than the files inside it.

```bash
ls -l /bin/login
```

Example output:

```text
-rwxr-xr-x 1 root root 19080 Apr 1 18:26 /bin/login
```

### Read Long-Listing Output

```text
-rwxr-xr-x 1 root root 19080 Apr 1 18:26 /bin/login
```

| Part | Meaning |
| --- | --- |
| `-` | File type. |
| `rwx` | Permissions for the file owner (user). |
| `r-x` | Permissions for the owning group. |
| `r-x` | Permissions for everyone else (others). |
| `1` | Number of hard links. |
| `root` | User owner. |
| `root` | Group owner. |
| `19080` | File size in bytes. |
| `Apr 1 18:26` | Last modification date and time. |
| `/bin/login` | File name and path. |

### File-Type Characters

| Character | File type |
| --- | --- |
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device, such as a keyboard |
| `b` | Block device, such as a hard disk |
| `s` | Socket |
| `p` | Named pipe |

### Permission Characters

| Character | Meaning for a file | Meaning for a directory |
| --- | --- | --- |
| `r` | Read the file. | List the directory contents. |
| `w` | Write to the file. | Create and remove files in the directory. |
| `x` | Execute a program. | Enter the directory and use a long listing. |
| `-` | Permission is not granted. | Permission is not granted. |

---

## Change File Ownership

- Only `root` can change a file's owner.
- Only `root` or the file owner can change a file's group.

### Change the Owner with `chown`

```bash
chown [-R] <USERNAME> <FILE_OR_DIRECTORY>
```

Use `-R` to apply the ownership change recursively to a directory and its contents.

### Change the Group with `chgrp`

```bash
chgrp [-R] <GROUP_NAME> <FILE_OR_DIRECTORY>
```

Use `-R` to apply the group change recursively to a directory and its contents.

---

## Change Permissions: Symbolic Method

Use `chmod` with symbols when you want to add, remove, or set specific permissions.

```bash
chmod [-OPTION] [mode] [+/-/=] [permission] <FILE_OR_DIRECTORY>
```

### Mode Characters

| Character | Meaning |
| --- | --- |
| `u` | User (owner) |
| `g` | Group |
| `o` | Others |
| `+` | Grant a permission |
| `-` | Remove a permission |
| `=` | Set permissions exactly |
| `r` | Read |
| `w` | Write |
| `x` | Execute |

### Useful `chmod` Options

| Option | Description |
| --- | --- |
| `-R` | Change permissions recursively. |
| `-v` | Show each file as it is processed. |
| `--reference=<REFERENCE_FILE>` | Use another file's permissions as the reference. |

### Examples

```bash
# Grant read access to the user, group, and others.
chmod ugo+r <FILE>

# Remove write and execute permissions from others on a directory.
chmod o-wx <DIRECTORY>
```

---

## Change Permissions: Numeric Method

The numeric method uses a three-digit mode number:

1. The first digit sets the owner's permissions.
2. The second digit sets the group's permissions.
3. The third digit sets permissions for others.

Calculate each digit by adding the values for the permissions you want:

| Permission | Value |
| --- | --- |
| Read (`r`) | `4` |
| Write (`w`) | `2` |
| Execute (`x`) | `1` |

### Example: `640`

```bash
chmod 640 <FILE>
```

| Who | Calculation | Result |
| --- | --- | --- |
| Owner | `4` (read) + `2` (write) | `6` (`rw-`) |
| Group | `4` (read) | `4` (`r--`) |
| Others | No permissions | `0` (`---`) |

This sets the file permissions to `rw-r-----`.
