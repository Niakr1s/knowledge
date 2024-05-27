# BTRFS

## Subvolumes

[Reference](https://btrfs.readthedocs.io/en/latest/Subvolumes.html)

A BTRFS subvolume is a part of filesystem with its own independent file/directory hierarchy.
A snapshot is also subvolume, but with a given initial content of the original subvolume.

A subvolume looks like a normal directory, with some additional operations described below.
Subvolumes can be renamed or moved.

A freshly created filesystem is also a subvolume, called top-level, internally has an id 5.
This subvolume cannot be removed or replaced by another subvolume.
This is also the subvolume that will be mounted by default, unless the default subvolume has been changed

A snapshot is a subvolume like any other, with given initial content.
By default, snapshots are created read-write.
File modifications in a snapshot do not affect the files in the original subvolume.

### Subvolume flags

The subvolume flag currently implemented is the ro property (read-only status). Read-write subvolumes have that set to false, snapshots as true.

Read-only snapshots are building blocks of incremental send.

### Nested subvolumes

There are no restrictions for subvolume creation, so it’s up to the user how to organize them, whether to have a flat layout (all subvolumes are direct descendants of the toplevel one), or nested.

What should be mentioned early is that a snapshotting is not recursive, so a subvolume or a snapshot is effectively a barrier and no files in the nested appear in the snapshot.
Instead there’s a stub (empty) subvolume.
This can be used intentionally but could be confusing in case of nested layouts.

## Compression

Btrfs supports transparent file compression. There are three algorithms available: zlib, lzo and zstd.

According to [benchmark](https://www.phoronix.com/review/btrfs-zstd-compress/4), zstd seems to be best choice.

### How to enable compression

Typically the compression can be enabled on the whole filesystem, specified for the mount point.
Note that the compression mount options are shared among all mounts of the same filesystem, either bind mounts or subvolume mounts.

```bash
mount -o compress=zstd:3 /dev/sdx /mnt
```

This can be added in `/etc/fstab`:

```bash
UUID=f4f83c00-161d-4ec1-888f-ae03376308af / btrfs defaults,compress=zstd:3 0 0
```

This command can be used to compress a single file:

```bash
btrfs filesystem defrag -czstd:3 file
```

This command can be used to compress a directory recursively:

```bash
btrfs filesystem defrag -r -v -czstd:3 directory
```

### Compression levels

- zlib: has 9 levels (1..9). The default is level 3, which provides the reasonably good compression ratio and is still reasonably fast.
- zstd: has 15 levels (1..15). The default is level 3. Levels 1-3 are real-time, 4-8 slower with improved compression and 9-15 try even harder though the resulting size may not be significantly improved.
-lzo: doesn't support levels (the kernel implementation provides only one).

Level 0 always maps to the default.

### Checking compression

Compression percentage cam be checked with a tool `compsize`:

```bash
compsize -x directory
```

Option `-x` is useful for root `/` directory (from documentation: `-x, --one-file-system   don't cross filesystem boundaries`).
