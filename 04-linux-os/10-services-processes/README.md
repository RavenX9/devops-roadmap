# Services and Processes

This lesson explains two related Linux ideas:

- **Services** are background programs managed by the operating system.
- **Processes** are running instances of programs, each identified by a process ID (PID).

---

## 1. Services in Linux

A **service** is a program that runs in the background. Unlike a program you open, use, and close, a service normally runs quietly behind the scenes and may start automatically during system boot.

Common examples include:

- A web server such as Apache (`httpd`) waiting for web requests
- The SSH service waiting for remote connections
- A firewall service monitoring network traffic

Services are also called **daemons**. Their names often end in `d`:

| Service name | Meaning |
|---|---|
| `sshd` | SSH daemon |
| `httpd` | Apache HTTP daemon |
| `firewalld` | Firewall daemon |

### Managing services with `systemctl`

On Linux systems that use `systemd`, use `systemctl` to manage services. It can start, stop, restart, enable, disable, and show the status of a service.

`systemctl` reads a service configuration file to determine how a service should be started, stopped, or reloaded. For example, an Apache service file can be located at:

```text
/usr/lib/systemd/system/httpd.service
```

When a package such as `httpd` is installed with `yum`, its service configuration file is normally created automatically.

### Start vs. enable

Starting and enabling a service are different actions:

| Action | What it does |
|---|---|
| **Start** | Runs the service now. It stops again after a reboot unless it is also enabled. |
| **Enable** | Configures the service to start automatically at boot. It does not start the service immediately. |

> **Important:** Starting a service does not enable it, and enabling a service does not start it. In many cases, both actions are needed.

### `systemctl` command reference

| Command | Description |
|---|---|
| `systemctl start httpd` | Start the Apache web service now. |
| `systemctl stop httpd` | Stop the Apache web service. |
| `systemctl restart httpd` | Restart the Apache web service. |
| `systemctl status httpd` | Show the current service status and related process information. |
| `systemctl enable httpd` | Configure Apache to start automatically at boot. |
| `systemctl disable httpd` | Prevent Apache from starting automatically at boot. |

> On Ubuntu, the Apache package and service are named `apache2` rather than `httpd`; the `systemctl` workflow is the same.

---

### How `systemctl` works behind the scenes

When you run a command such as:

```bash
systemctl start httpd
```

Linux first reads the service configuration file. A `.service` file is the service's instruction manual. It can define:

- The command used to start the service
- The command used to stop the service
- The user that should run the service
- When the service should start, such as at boot or manually

For example, when `systemctl start httpd` is run, `systemctl` reads the service file, checks its `ExecStart=` instruction, and runs the configured command. This lets you manage services with one consistent command instead of memorizing each service's longer startup command.

### Where `.service` files come from

Installing a package with a command such as:

```bash
yum install httpd
```

usually installs its `.service` file automatically. Standard locations include:

```text
/lib/systemd/system/     # default installed services
/etc/systemd/system/     # user-defined service files or overrides
```

If software is downloaded manually, such as from a tar archive, no service file is automatically created. In that case, you can create a `.service` file so that `systemctl` knows how to manage the software.

### What is `multi-user.target.wants`?

When you enable a service, `systemctl` creates a symbolic link in a directory such as:

```text
/etc/systemd/system/multi-user.target.wants/
```

That link points to the actual service file, for example:

```text
/usr/lib/systemd/system/httpd.service
```

`multi-user.target` is a normal server boot stage: the system is fully booted, networking is available, and multiple users can log in. The `wants` directory lists services that should start when Linux reaches that stage.

In practice:

1. `systemctl enable httpd` creates a link in `multi-user.target.wants/`.
2. During boot, Linux reaches `multi-user.target`, finds `httpd.service`, and starts it.
3. `systemctl disable httpd` removes the link so the service no longer starts automatically at boot.

---

## 2. Processes in Linux

Every time you run a command or start a program, Linux creates a **process**. A process is a running instance of a program.

For example:

- Running `systemctl start httpd` starts the Apache web server, which becomes a running process.
- A command typed in a terminal is a process while it is executing.

Every process receives a unique **PID** (**Process ID**). Linux uses PIDs to keep track of running work. The PIDs shown by `systemctl status httpd` belong to the service's running processes.

### Types of processes

| Type | Description |
|---|---|
| **Foreground process** | Runs directly in the terminal. You wait for it to finish before entering another command. |
| **Background process** | Runs behind the scenes without blocking the terminal. A service such as `httpd` is an example. |

### Parent and child processes

When one process starts another process, the original process is the **parent** and the new process is the **child**. For example, when `systemctl` starts `httpd`, the web server can create child processes to handle incoming requests. This is why `systemctl status httpd` can show more than one PID.

---

### Viewing processes

#### `ps aux`

```bash
ps aux
```

This command provides a snapshot of all currently running processes. Its output includes details such as the user running the process, PID, CPU and memory usage, and the command that started it.

To filter the list for a specific process, combine it with `grep`:

```bash
ps aux | grep httpd
```

This shows running processes related to `httpd`.

The first process that starts when Linux boots is `systemd` (or `init` on older systems). It has PID `1`, and other processes are descendants of it.

#### `top`

```bash
top
```

`top` is a real-time process-monitoring tool. Unlike the static output from `ps aux`, it continuously updates CPU usage, RAM consumption, system uptime, and load average.

| Key | Action |
|---|---|
| `q` | Exit `top`. |
| `k` | Kill a process from inside `top`. |

While using `top` or `ps aux`, you can see process states:

| State | Meaning |
|---|---|
| Running | Actively using the CPU. |
| Sleeping | Waiting for something, such as input or a resource. |
| Stopped | Paused by a signal. |
| Zombie | Finished running but still present in the process table. |

A **zombie process** has completed but has not been fully cleaned up by its parent. It does not consume normal runtime resources, but it occupies an entry in the process table. The source lesson identifies rebooting the machine as the most common fix.

An **orphan process** no longer has its original parent because that parent ended; it is adopted by the system (`init`).

#### `ps -ef`

```bash
ps -ef
```

Like `ps aux`, this command gives a static snapshot of running processes, but its output is useful for tracing process relationships.

| Column | Meaning |
|---|---|
| `UID` | User who owns the process. |
| `PID` | Process ID. |
| `PPID` | Parent Process ID. |
| `CMD` | Command that started the process. |

`PPID` is especially useful because it identifies the process that created the current one. Use `ps aux` for a general view of processes and resource use; use `ps -ef` when you need to trace parent-child relationships.

---

### Stopping processes

Use `kill` with a PID to terminate a process:

```bash
kill <PID>
```

If a process does not respond, the source lesson gives a forceful option:

```bash
kill -9 <PID>
```

> **Use caution:** `kill -9` forcefully terminates the selected parent process but does not automatically terminate its child processes. This can leave orphan processes behind.

### Filtering and managing multiple processes

The lesson shows two ways to target processes related to a service:

```bash
ps aux | grep httpd | awk '{print $2}' | xargs kill
```

```bash
pkill -9 httpd
```

> **Use caution:** These commands can terminate multiple processes. Confirm the service name and the PIDs you intend to target before running them.

### Understanding `xargs`

`xargs` takes input and passes it to another command as arguments. It is helpful when the next command does not read input directly from a pipe.

For example, this does **not** work because `kill` expects PID arguments directly:

```bash
ps aux | grep httpd | awk '{print $2}' | kill
```

`xargs` bridges the pipeline and `kill`:

```bash
ps aux | grep httpd | awk '{print $2}' | xargs kill
```

Read it as: find the `httpd` PIDs, then pass each PID to `kill` one at a time.

The same pattern can be used with other commands:

```bash
find . -name "*.log" | xargs rm
```

```bash
find . -name "*.txt" | xargs grep "error"
```

The first command finds `.log` files and passes them to `rm`; the second finds `.txt` files and searches each one for `error`.

> **Safety note:** The `rm` example deletes files. Review the list returned by `find` before using a deletion command.

---

## Quick recap

| Topic | Key idea |
|---|---|
| Service | A background program, often managed by `systemctl`. |
| Daemon | Another name for a service; names often end in `d`. |
| Start | Run a service now. |
| Enable | Configure a service to run automatically at boot. |
| Process | A running instance of a program. |
| PID | Unique number that identifies a process. |
| PPID | PID of the process's parent. |
| `ps aux` | Static view of running processes and resource usage. |
| `top` | Real-time process monitor. |
| `ps -ef` | Static process view that highlights parent-child relationships. |
