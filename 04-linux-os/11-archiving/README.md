# Archiving

## Archiving in Linux

Archiving bundles files and directories into one file for backup, transfer, or storage. This lesson introduces two common tools:

- `tar` — a long-standing Linux archiving tool with many options.
- `zip` / `unzip` — a simpler, cross-platform option that may feel familiar to Windows and macOS users.

---

## `tar` (Tape Archive)

`tar` is a widely used Linux archiving tool. A compressed `tar` archive is often called a **tarball**.

### Create a compressed tarball

```bash
tar -czvf <ARCHIVE_NAME>.tar.gz <SOURCE_DIRECTORY>
```

| Option | Meaning |
|---|---|
| `-c` | Create a new archive. |
| `-z` | Compress the archive with `gzip`. |
| `-v` | Show verbose progress. |
| `-f` | Specify the output filename. |

The `.tar.gz` extension identifies a compressed tarball. The `.gz` portion means that `gzip` was used for compression.

### Extract a tarball

```bash
tar -xzvf <ARCHIVE_NAME>.tar.gz
```

To extract the archive into a specific directory, use `-C`:

```bash
tar -xzvf <ARCHIVE_NAME>.tar.gz -C <DESTINATION_DIRECTORY>
```

### Other `tar` compression options

| Option | Compression method |
|---|---|
| `-z` | `gzip` |
| `-j` | `bzip2` |
| `-J` | `xz` |
| `-a` | Automatically detect the compression method from the file extension. |

`tar` can also compare two tarballs with `-d` or update an existing tarball. For example:

```bash
tar -d <ARCHIVE_1>.tar <ARCHIVE_2>.tar
```

---

## `zip` and `unzip`

`zip` is a simpler archiving format. Install `zip` and `unzip` before using them:

```bash
yum install zip unzip -y
```

### Create a ZIP archive

General syntax:

```bash
zip [options] [destination_name.zip] [source_folder]
```

Example pattern for archiving a directory:

```bash
zip -r <ARCHIVE_NAME>.zip <SOURCE_DIRECTORY>
```

The `-r` option is required when archiving a directory; it means **recursive**.

### Extract a ZIP archive

```bash
unzip <ARCHIVE_NAME>.zip
```

---

## Quick comparison

| Tool | Command pattern | Notes |
|---|---|---|
| `tar` | `tar -czvf <ARCHIVE_NAME>.tar.gz <SOURCE_DIRECTORY>` | A feature-rich, long-standing Linux tool. |
| `zip` | `zip -r <ARCHIVE_NAME>.zip <SOURCE_DIRECTORY>` | Simpler and cross-platform. |

A common archiving use case is old log files: archive and compress them to free disk space, then move the archive to another storage location.
