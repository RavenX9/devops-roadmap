# Users & Groups

Linux uses users and groups to control access to files and resources.

- Users sign in with a username and password.
- Each file has an owning user and an associated group.
- Each process also has an owner and group affiliation. It can access only the resources allowed to that user or group.
- Each user has a unique **UID** (user ID).
- User names and UIDs are stored in `/etc/passwd`; password information is stored in encrypted form in `/etc/shadow`.
- Users have a home directory and a login program, usually a shell.
- A user cannot read (`r`), write (`w`), or execute (`x`) another user's files without permission.

---

## Types of users

| Type | Example | UID | GID | Home directory | Shell |
| --- | --- | --- | --- | --- | --- |
| Root | `root` | `0` | `0` | `/root` | `/bin/bash` |
| Regular | `<USERNAME>` | `1000` to `6000` | `1000` to `6000` | `/home/<USERNAME>` | `/bin/bash` |
| Service | `ftp`, `ssh`, `apache` | `1` to `999` | `1` to `999` | `/var/ftp` | `/sbin/nologin` |

---

## `/etc/passwd` file

Use `cat /etc/passwd` to list system users. To inspect the first entry only, use:

```bash
head -1 /etc/passwd
```

Example entry:

```text
root:x:0:0:root:/root:/bin/bash
```

An `/etc/passwd` entry contains seven colon-separated fields:

| Field | Example value | Meaning |
| --- | --- | --- |
| Username | `root` | The account name. |
| Shadow-file link | `x` | Indicates that password information is stored in the shadow file. |
| User ID | `0` | The UID. |
| Group ID | `0` | The primary GID. |
| Comment | `root` | A comment or descriptive field. |
| Home directory | `/root` | The user's home directory. |
| Login shell | `/bin/bash` | The program started at login. |

## `/etc/group` file

The `/etc/group` file contains group information.

---

## Inspect a user's identity

Use `id` followed by a username to display that account's UID, GID, and group memberships:

```bash
id <USERNAME>
```

Example output format:

```text
uid=1000(<USERNAME>) gid=1000(<USERNAME>) groups=1000(<USERNAME>)
```

---

## Create users with `useradd`

`useradd` creates a user account.

```bash
useradd <USERNAME>
```

After adding users, their entries appear in `/etc/passwd`. The source also shows that corresponding groups are created automatically when the users are added.

```text
<USERNAME>:x:1001:1001::/home/<USERNAME>:/bin/bash
```

```text
<USERNAME>:x:1001:
```

---

## Create groups with `groupadd`

`groupadd` creates a group on a Linux system.

```bash
groupadd [options] <GROUPNAME>
```

Example:

```bash
groupadd devops
```

Verify that the group was created:

```bash
grep devops /etc/group
```

---

## Modify users with `usermod`

Creating a group does not automatically add users to it. `usermod` changes properties of an existing user account, such as its group membership.

```bash
usermod [options] <USERNAME>
```

### Add a user to a supplementary group

```bash
usermod -aG devops <USERNAME>
```

Here, `-aG` appends the user to the supplementary `devops` group.

### Rename a login

```bash
usermod -l <NEW_USERNAME> <CURRENT_USERNAME>
```

This changes the login name from `<CURRENT_USERNAME>` to `<NEW_USERNAME>`.

### Alternative shown in the source: edit `/etc/group` in Vim

The source also shows adding users by editing a group entry directly:

```bash
vim /etc/group
```

For example, an entry such as:

```text
devops:x:1004:<USERNAME>
```

can be changed to:

```text
devops:x:1004:<USERNAME>,<ANOTHER_USERNAME>,<THIRD_USERNAME>
```

Then save and quit Vim with:

```vim
:wq
```

---

## Delete users with `userdel`

`userdel` deletes a user account.

```bash
userdel <USERNAME>
```

This removes account information from files such as `/etc/passwd` and `/etc/shadow`, but does not remove the user's home directory by default. To remove the account and its home directory:

```bash
userdel -r <USERNAME>
```

---

## Delete groups with `groupdel`

`groupdel` deletes a group.

```bash
groupdel <GROUPNAME>
```

This removes the group from `/etc/group`.

> **Common issue:** Linux normally does not allow deletion of a group that is a user's primary group. Change the user's primary group or delete the user first.

---

## Manage passwords with `passwd`

`passwd` sets or changes a user's password. A user can change their own password, and the root user can change passwords for other users. The source also notes that root can switch to another user's account without supplying that user's password:

```bash
su <USERNAME>
```

Use this syntax to set or change a password:

```bash
passwd [<USERNAME>]
```

### Common password commands

| Task | Command |
| --- | --- |
| Change your own password | `passwd` |
| Unlock an account | `passwd -u <USERNAME>` |
| Lock an account | `passwd -l <USERNAME>` |
| Check password status | `passwd -S <USERNAME>` |

When a status result contains `LK`, the password is locked.

---

## Review login activity with `last`

`last` displays login history. It reads login records (typically `/var/log/wtmp`) and shows who logged in, when, from where, and for how long.

```bash
last
```

A redacted example of the output format:

```text
<USERNAME> pts/0 <SERVER_IP> Mon Jun 1 09:15 still logged in
root       pts/1 <SERVER_IP> Mon Jun 1 08:50 - 09:10 (00:20)
reboot     system boot 5.14.0-... Mon Jun 1 08:00
```

---

## Show current users with `who`

`who` shows users who are currently logged in.

```bash
who
```

---

## List open files with `lsof`

`lsof` means **list open files**. In Linux, many resources are treated as files, including:

- Regular files
- Directories
- Network sockets
- Disks
- Pipes
- Terminals
