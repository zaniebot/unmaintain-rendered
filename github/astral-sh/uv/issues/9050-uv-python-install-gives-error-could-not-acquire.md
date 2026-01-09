---
number: 9050
title: "`uv python install` gives `error: Could not acquire lock for '.local/share/uv/python' at '.local/share/uv/python/.lock': Function not implemented (os error 38)`"
type: issue
state: closed
author: maximilianigl
labels: []
assignees: []
created_at: 2024-11-12T09:33:53Z
updated_at: 2025-02-12T15:49:54Z
url: https://github.com/astral-sh/uv/issues/9050
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv python install` gives `error: Could not acquire lock for '.local/share/uv/python' at '.local/share/uv/python/.lock': Function not implemented (os error 38)`

---

_Issue opened by @maximilianigl on 2024-11-12 09:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When running `uv python install`, I get the error 
```
error: Could not acquire lock for `.local/share/uv/python` at `.local/share/uv/python/.lock`: Function not implemented (os error 38)
```

It's on a linux cluster node (running Slurm), so the home directory is shared across nodes.
`uv` version is 0.5.1

---

_Comment by @zanieb on 2024-11-12 13:49_

Does your system not support [`flock`](https://linux.die.net/man/2/flock)? We need to be able to take locks to prevent concurrent access to state.

---

_Comment by @maximilianigl on 2024-11-12 14:14_

Thanks for the prompt response! 
I didn't realize it was related to `flock`. They recently (temporarily?) disabled it.
I'll close the issue as this is a problem on our system.

---

_Closed by @maximilianigl on 2024-11-12 14:14_

---

_Comment by @zanieb on 2024-11-12 14:20_

Yeah the "Function not implemented" is the error from the OS when flock is invoked.

---

_Comment by @gashon on 2025-01-07 21:47_

I was able to move .lock location w `export UV_PYTHON_INSTALL_DIR=/tmp/uv_python_install` (can just move to fs that supports flock)

---

_Comment by @mil-ad on 2025-02-12 15:21_

@zanieb does this mean that it's safe to call `uv sync` concurrently from multiple processes if the filesystem supports flock?

And apologies for commenting on a closed thread.

---

_Comment by @zanieb on 2025-02-12 15:22_

Yes we lock environments for safe concurrency.

---

_Comment by @mil-ad on 2025-02-12 15:26_

Amazing! I think flock doesn't work on NFS but [fcntl](https://man7.org/linux/man-pages/man2/fcntl.2.html) does, at least for more recent versions of NFS and linux kernel

---

_Comment by @zanieb on 2025-02-12 15:42_

I think we only use `flock` right now. Maybe worth opening a new issue tracking a different locking mechanism?

For reference

https://github.com/astral-sh/uv/blob/711766e79c11602ce39506693d17ddeefa4bfe85/crates/uv-fs/src/lib.rs#L564

https://tikv.github.io/doc/fs2/trait.FileExt.html#tymethod.try_lock_exclusive

---

_Referenced in [astral-sh/uv#13626](../../astral-sh/uv/issues/13626.md) on 2025-05-23 20:42_

---
