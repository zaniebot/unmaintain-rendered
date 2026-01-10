---
number: 21
title: Explore use of alternative file-copy APIs
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-06T05:16:47Z
updated_at: 2023-10-08T04:04:50Z
url: https://github.com/astral-sh/uv/issues/21
synced_at: 2026-01-10T01:23:03Z
---

# Explore use of alternative file-copy APIs

---

_Issue opened by @charliermarsh on 2023-10-06 05:16_

Orogene, Bun, pnpm, etc. all have per-platform optimizations for file-copying.

https://twitter.com/zkat__/status/1701289288296702464


---

_Comment by @charliermarsh on 2023-10-07 13:19_

- A filename is an entry in a directory file.
- The entry contains the file name, and the inode number (indexer into the inode table).
- Inode table entries contain file attributes, plus pointers to the data.
- Hard link: two directory entries with the same inode number. (Only works within a single mounted volume.)
- Symbolic link: special file where the attributes contains a pathname specifying the target of the link. (Works across volumes.)
- The downside to hard links and symbolic links (for caches) is that if you edit the symlink, you pollute the cache.
- Reflinks: copy-on-write links. Unlike a hard link, you have two inode entries, and once modified, the data is copied.
- Reflinks are only supported on some operating systems.

---

_Comment by @charliermarsh on 2023-10-07 13:19_

This is what Orogene seems to use: https://github.com/cargo-bins/reflink-copy

---

_Comment by @charliermarsh on 2023-10-07 13:21_

Oh, `cacache` already supports this, amazing.

---

_Comment by @charliermarsh on 2023-10-07 15:12_

So in Orogene, they download tarballs from NPM. They store the tarball index in the entry metadata, where the entry is keyed by the tarball hash (from NPM). They then store each file from the tarball in the cache, using a hash that's computed during extraction.

When they copy from the cache, they fetch the index, iterate over it, and copy each file using its hash.


---

_Referenced in [astral-sh/uv#49](../../astral-sh/uv/pulls/49.md) on 2023-10-08 03:53_

---

_Closed by @charliermarsh on 2023-10-08 04:04_

---
