```yaml
number: 8886
title: Build basic source distributions
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/uv-build-backend-source-dist
created_at: 2024-11-07T13:02:15Z
updated_at: 2024-11-08T09:16:34Z
url: https://github.com/astral-sh/uv/pull/8886
synced_at: 2026-01-12T16:08:32Z
```

# Build basic source distributions

---

_@konstin_

Very basic source distribution support. What's included:

- Include and exclude patterns (hard-coded): Currently, we have globset+walkdir in one part and glob in the other. I'll migrate everything to globset+walkset and some custom perf optimizations to avoid traversing irrelevant directories on top. I'll also pick a glob syntax (or subset), PEP 639 seems like a good candidate since it's consistent with what we already have to support.
- Add the `PKG-INFO` file with metadata: Thanks to Core Metadata 2.2, this metadata is reliable and can be read statically by external tools.

Example output:

```
$ tar -ztvf dist/dummy-0.1.0.tar.gz
-rw-r--r-- 0/0             154 1970-01-01 01:00 dummy-0.1.0/PKG-INFO
-rw-rw-r-- 0/0             509 1970-01-01 01:00 dummy-0.1.0/pyproject.toml
drwxrwxr-x 0/0               0 1970-01-01 01:00 dummy-0.1.0/src/dummy
drwxrwxr-x 0/0               0 1970-01-01 01:00 dummy-0.1.0/src/dummy/submodule
-rw-rw-r-- 0/0              30 1970-01-01 01:00 dummy-0.1.0/src/dummy/submodule/impl.py
-rw-rw-r-- 0/0              14 1970-01-01 01:00 dummy-0.1.0/src/dummy/submodule/__init__.py
-rw-rw-r-- 0/0              12 1970-01-01 01:00 dummy-0.1.0/src/dummy/__init__.py
```

No tests since the source distributions don't build valid wheels yet.

---

_Review requested from @BurntSushi by @konstin on 2024-11-07 13:02_

---

_@konstin reviewed on 2024-11-07 13:02_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:386 on 2024-11-07 13:02_

It's not fully clear what the right default here is

---

_@konstin reviewed on 2024-11-07 13:03_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:441 on 2024-11-07 13:03_

It's not fully clear what the right default here is, since we're e.g. writing source dists that get used on unix

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:386 on 2024-11-07 13:11_

I think this seems fine to me generally speaking. Or is there a more specific concern you have?

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:441 on 2024-11-07 13:13_

I'm not even sure what this means on non-Unix?

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:415 on 2024-11-07 13:14_

I believe this is correct.

---

_@BurntSushi approved on 2024-11-07 13:15_

Looks like a good start!

---

_@konstin reviewed on 2024-11-07 13:29_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:441 on 2024-11-07 13:29_

The problem we want to avoid is that we're writing a `.tar.gz` on windows, setting a default 000 mode and then fail to read the unpacked files on unix.

There are other odd implications, e.g., when you check out a git repo and build a source dist from it, it may be different when built on windows and when built on unix: On windows, we lose the permissions that may be stored in git (as ntfs does not support them), while on unix, we copy the permissions to the archive.

In the wild, 644 and 755 seem to be the most popular defaults: https://github.com/search?q=header.set_mode&type=code

---

_Label `preview` added by @konstin on 2024-11-07 13:29_

---

_Merged by @konstin on 2024-11-07 13:29_

---

_Closed by @konstin on 2024-11-07 13:29_

---

_Branch deleted on 2024-11-07 13:29_

---

_@BurntSushi reviewed on 2024-11-07 13:52_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:441 on 2024-11-07 13:52_

Yeah, and I imagine at least on Unix, a user's umask will smooth things out even when `644` is undesirable.

---

_@harkabeeparolus reviewed on 2024-11-08 07:06_

---

_Review comment by @harkabeeparolus on `crates/uv-build-backend/src/lib.rs`:441 on 2024-11-08 07:06_

[Here's what bsdtar / libarchive does on Windows](https://github.com/libarchive/libarchive/blob/d60f3ea1b3bfd9f1dcd4040de9fca53fbedd829a/libarchive/archive_read_disk_windows.c#L2035C9-L2035C36). My C is a bit _rusty_ (hehe 🙄), but basically:

1. Start with read bits for everyone, i.e. user, group and other: 444
2. If the Windows file/dir is not readonly, add write bits for everyone (with bitwise OR): 222 (resulting in 666)
3. If it is a directory, add execute bits for everyone: 111
4. If it is a regular file ending in *.bat, *.cmd, or *.exe, add execute bits for everyone: 111

I feel like there ought to be a portable Rust crate for creating tar files that performs reasonably similar heuristics on Windows, if you don't have a strong desire to roll your own heuristics. 😊

This will work fine while running as a normal user (non-root), since tar by default applies the normal user's umask when extracting a tar archive. From the [manual page](https://www.gnu.org/software/tar/manual/html_node/Option-Summary.html#index-no_002dsame_002dpermissions_002c-summary):

> ‘--no-same-permissions’
> When extracting an archive, subtract the user’s umask from files from the permissions specified in the archive. **This is the default behavior for ordinary users**.

[And](https://www.gnu.org/software/tar/manual/html_node/Attributes.html#index-preserve_002dpermissions_002c-short-description):

> ‘-p’
> ‘--same-permissions’
> ‘--preserve-permissions’
> Extract all protection information.
> 
> This option causes tar to set the modes (access permissions) of extracted files exactly as recorded in the archive. If this option is not used, the current umask setting limits the permissions on extracted files. This option is by default enabled when tar is executed by a superuser.

If someone does extract a random tar file from the Internet as root, they should know to use `--no-same-permissions` and similar flags, in order not to shoot themselves in the foot.

---
