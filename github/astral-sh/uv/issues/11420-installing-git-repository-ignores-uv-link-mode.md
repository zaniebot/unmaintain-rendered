---
number: 11420
title: installing git repository ignores UV_LINK_MODE, breaks on some filesystems
type: issue
state: closed
author: PhilipVinc
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-02-11T12:53:32Z
updated_at: 2025-02-12T00:42:15Z
url: https://github.com/astral-sh/uv/issues/11420
synced_at: 2026-01-07T13:12:18-06:00
---

# installing git repository ignores UV_LINK_MODE, breaks on some filesystems

---

_Issue opened by @PhilipVinc on 2025-02-11 12:53_

### Summary

I am on a cluster with a distributed filesystem (beefgs) which does not support hard links.
I am trying to install a repository directly, but it fails when symlinking the repository between two locations in the uv cache directory.

I would assume that setting `UV_LINK_MODE=symlink` or `UV_LINK_MODE=copy` would fix the error, but it seems that the setting is not applied to the git command in this case

```bash
(base) [filippo.vicentini@cholesky-login02 prova2]$ uv init --no-package
Initialized project `prova2`
(base) [filippo.vicentini@cholesky-login02 prova2]$ uv add git+https://github.com/netket/netket
error: Git operation failed
  Caused by: process didn't exit successfully: `/usr/bin/git clone --local /mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/db/292f78b3ecf26bf6 /mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/checkouts/292f78b3ecf26bf6/f1db6bb` (exit status: 128)
--- stdout
Cloning into '/mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/checkouts/292f78b3ecf26bf6/f1db6bb'...

--- stderr
fatal: failed to create link '/mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/checkouts/292f78b3ecf26bf6/f1db6bb/.git/objects/pack/pack-384b576a7a115ef20196172649a6bf2ab2b2ab8e.idx': Operation not permitted
fatal: The remote end hung up unexpectedly
(base) [filippo.vicentini@cholesky-login02 prova2]$ ^C
(base) [filippo.vicentini@cholesky-login02 prova2]$ export UV_LINK_MODE=copy
(base) [filippo.vicentini@cholesky-login02 prova2]$ uv add git+https://github.com/netket/netket
error: Git operation failed
  Caused by: process didn't exit successfully: `/usr/bin/git clone --local /mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/db/292f78b3ecf26bf6 /mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/checkouts/292f78b3ecf26bf6/f1db6bb` (exit status: 128)
--- stdout
Cloning into '/mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/checkouts/292f78b3ecf26bf6/f1db6bb'...

--- stderr
fatal: failed to create link '/mnt/beegfs/home/CPHT/filippo.vicentini/.cache/uv/git-v0/checkouts/292f78b3ecf26bf6/f1db6bb/.git/objects/pack/pack-384b576a7a115ef20196172649a6bf2ab2b2ab8e.idx': Operation not permitted
fatal: The remote end hung up unexpectedly
```



### Platform

Linux 3.10.0-1062.el7.x86_64 x86_64 GNU/Linux

### Version

uv 0.5.30

### Python version

Python 3.12.7

---

_Label `bug` added by @PhilipVinc on 2025-02-11 12:53_

---

_Comment by @charliermarsh on 2025-02-11 13:27_

Hmm, we'll need to decide whether we want to support this. This is a link _within_ the cache, which is not what `UV_LINK_MODE` is intended to affect.

---

_Label `needs-decision` added by @charliermarsh on 2025-02-11 13:27_

---

_Comment by @PhilipVinc on 2025-02-11 13:47_

I just will point out that this kind of filesystem/restriction is relatively common in academic clusters.
If you do not support this, it becomes impossible to install a git repository, which are very common to keep private research code, and we don't necessarily tag/build wheels (it's very hard to setup a private wheel depot).

I hope you'd consider supporting this in some way, as otherwise we'd be forced to use other tools in academic environments.
I find that uv helps students/researchers have a much more reproducible environment, and save them time, so I would hope I can keep pushing for it (but due to #7642 / #1495 it is hard...).

So please, do support this kind of setups!


---

_Comment by @charliermarsh on 2025-02-11 14:24_

I think it makes sense to re-try the operation with `--no-hardlinks` if it fails.

---

_Label `bug` removed by @charliermarsh on 2025-02-11 14:24_

---

_Label `help wanted` added by @charliermarsh on 2025-02-11 14:24_

---

_Label `needs-decision` removed by @charliermarsh on 2025-02-11 14:24_

---

_Label `bug` added by @charliermarsh on 2025-02-11 14:24_

---

_Referenced in [astral-sh/uv#11421](../../astral-sh/uv/pulls/11421.md) on 2025-02-11 14:47_

---

_Assigned to @Gankra by @charliermarsh on 2025-02-11 19:48_

---

_Closed by @charliermarsh on 2025-02-12 00:42_

---

_Closed by @charliermarsh on 2025-02-12 00:42_

---
