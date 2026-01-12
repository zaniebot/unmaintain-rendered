```yaml
number: 1944
title: Avoid erroring for source distributions with symlinks in archive
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/sym
created_at: 2024-02-23T20:47:44Z
updated_at: 2024-02-24T03:22:15Z
url: https://github.com/astral-sh/uv/pull/1944
synced_at: 2026-01-12T16:04:48Z
```

# Avoid erroring for source distributions with symlinks in archive

---

_@charliermarsh_

## Summary

For context, we have three extraction paths:

- untar (async) - used for any `.tar.gz`, local or remote.
- unzip (async) - used to unzip remote wheels, or local or remote source distributions.
- unzip (sync) - used to untar locally-available wheels into the cache.

We use three different crates for these:

- [`tokio-tar`](https://github.com/vorot93/tokio-tar)
- [`async-zip`](https://github.com/Majored/rs-async-zip)
- [`zip-rs`](https://github.com/zip-rs/zip)

These all seem to have different support for symlinks:

- `tokio-tar` tries to create a symlink (which works fine on Unix but errors on Windows, since we typically don't have elevated permissions).
- `async-zip` _seems_ to write the target contents directly to the file (which is what we want).
- `zip-rs` _apparently_ writes the _name_ of the target to the file (which isn't what we want).

Thankfully, symlinks are not allowed in wheels (https://github.com/pypa/pip/issues/5919, https://discuss.python.org/t/symbolic-links-in-wheels/1945), so we can ignore `zip-rs`.

For `tokio-tar`, we now _skip_ (and warn) if we see a symlink on Windows. We could do what pip does, and recursively copy, but it's difficult because we don't have `Seek` on the file. (Alternatively, we could use hard links and junctions, though those also might need to exist already.) Let's see how far this gets us.

(We also no longer attempt to set permissions on symlinks on Unix, which caused another failure.)

Closes https://github.com/astral-sh/uv/issues/1858.


---

_Label `bug` added by @charliermarsh on 2024-02-23 20:47_

---

_Review requested from @konstin by @charliermarsh on 2024-02-23 20:52_

---

_Marked ready for review by @charliermarsh on 2024-02-23 20:53_

---

_Comment by @charliermarsh on 2024-02-23 21:00_

I need to test this with a symlinked directory before merging.

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:117 on 2024-02-23 22:30_

Can you make this `if cfg!(windows)` so the branch gets checked when compiling on unix?

---

_@konstin approved on 2024-02-23 22:32_

---

_Comment by @charliermarsh on 2024-02-24 03:18_

Okay, I mean, weird things happen if you include a symlink directory. On macOS, if you run create a symlink directory in `pgpdump-1.5`, then run `zip -r pgpdump-1.5.zip pgpdump-1.5`, then unzip, the symlink directory is just a file (not a symlink, and not a directory). If you compress and uncompress in the Finder UI, the symlink directory is preserved, but `async-zip` fails to unzip it with `feature not supported: 'stream reading entries with data descriptors (planned to be reintroduced)'`. Meanwhile, `tokio-tar` will untar it, but it too gets represented as a file.

---

_Merged by @charliermarsh on 2024-02-24 03:22_

---

_Closed by @charliermarsh on 2024-02-24 03:22_

---

_Branch deleted on 2024-02-24 03:22_

---
