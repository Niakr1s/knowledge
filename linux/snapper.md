# [Snapper](https://en.opensuse.org/openSUSE:Snapper_Tutorial)

Snapper is a tool for filesystem snapshot management for `btrfs` filesystem.

## Getting snapshot information

### List all snapshots

```bash
snapper -c root list
```

### Show which files and directories have been changed between snapshots

```bash
snapper status -c root 21..22
snapper status -c root 41..39
```

### Show the diff between snapshots

- Show full diff:

```bash
snapper diff -c root 21..22
snapper diff -c root 41..39
```

- Show diff between a specific file:

```bash
snapper diff -c root 21..22 /etc/zypp/zypp.conf
```

## Managing snapshots

### Cleanup Algorithms

Unless you have a good reason to do otherwise, you should always specify the cleanup algorithm when taking a snapshot, otherwise the snapshot will never be deleted unless you do it manually.
You do this by adding the following to your snapper commands when creating snapshots:
`--cleanup-algorithm <number|timeline|empty-pre-post>`

### Create one snapshot

```bash
snapper -c home create --description "description"
```

### Create one snapshot with cleanup algorithm

```bash
snapper create -c home --description "description" --cleanup-algorithm number
```

### Create pre and post snapshots

Takes a snapshot of the type pre and prints the snapshot number.
First command needed to take a pair of snapshots used to save a "before" and "after" state:

```bash
snapper create --type pre --print-number --description "Before change" --cleanup-algorithm number
```

Takes a snapshot of the type post paired with the pre snapshot number 30.
Second command needed to take a pair of snapshots used to save a "before" and "after" state:

```bash
snapper create --type post --pre-number 30 --description "After change" --cleanup-algorithm number
```

### Deleting snapshots

To delete snapshot #64 for `root` configuration (gotten with command `list`):

```bash
snapper delete -c root 64
```

If you want to instead delete all snapshots, first do `snapper list` as `root` user, then replace 100000 by the highest snapshot number:

```bash
snapper delete 1-100000
```

## Managing configurations

### Create configuration

To create configuration `foo`:

```bash
snapper -c foo create-config /foo
```

What really happens, when this command is put?

- Configuration file `/etc/snapper/configs/foo` is created.
- Folder, where snapshots for `/foo` subvolume will be stored: `/foo/.snapshots/` , is created.
- Configuration file `/etc/sysconfig/snapper` is updated by changing `SNAPPER_CONFIGS` parameter: `SNAPPER_CONFIGS="foo"`.

### Configuration settings

You can get settings for configuration `home`  with command:

```bash
snapper -c root get-config
```

You can set a setting with command with `set-config` option, for example:

```bash
snapper -c root set-config TIMELINE_LIMIT_DAILY=10
```

### Delete configuration

To delete configuration `foo`:

```bash
snapper -c foo delete-config
```

What really happens, when this command is put?

- Configuration file `/etc/snapper/configs/foo` is deleted.
- Folder, where snapshots for `/foo` subvolume is stored: `/foo/.snapshots/` , is deleted.
- Configuration file `/etc/sysconfig/snapper` is updated by changing `SNAPPER_CONFIGS` parameter: `SNAPPER_CONFIGS=""` (foo record is deleted).
