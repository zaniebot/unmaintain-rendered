```yaml
number: 15888
title: "Make `uv cache clean` parallel process safe"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/safe-cache-clean
created_at: 2025-09-16T08:49:46Z
updated_at: 2025-11-07T15:19:08Z
url: https://github.com/astral-sh/uv/pull/15888
synced_at: 2026-01-12T16:12:00Z
```

# Make `uv cache clean` parallel process safe

---

_@konstin_

Currently, `uv cache clean` and `uv cache prune` can cause crashes in other uv processes running in parallel by removing their in-use files.

We can solve this by using a shared (read) lock on the cache directory, while the `uv cache` operations use an exclusive (write) lock. The drawback is that this is always one extra lock, and that we assume that all platforms support shared locks.

Once Rust 1.89 fulfills our N-2 policy, we can add support for these methods in fs_err and switch to https://doc.rust-lang.org/std/fs/struct.File.html#platform-specific-behavior-2.

**Test Plan**

Open one terminal, run:

```
uv venv -c -p 3.13
UV_CACHE_DIR=cache uv cache clean
UV_CACHE_DIR=cache uv pip install numpy==2.0.0
```

Open another terminal, run:

```
UV_CACHE_DIR=cache uv cache clean
```

Fixes #15704
Part of #13883

---

_Label `bug` added by @konstin on 2025-09-16 08:49_

---

_Comment by @zanieb on 2025-09-17 10:52_

>  and that we assume that all platforms support shared locks.

How confident are you in this?

---

_Comment by @konstin on 2025-09-17 11:18_

Given that there's support for this in the Rust standard library without warnings about platforms that don't support this, I expect wide support. I added a path that ignores platforms and filesystems with missing shared lock support.

CC @BurntSushi who knows about file locking and @geofft who knows about Unix.

---

_Comment by @geofft on 2025-09-17 12:17_

The docs say
> This function currently corresponds to the `flock` function on Unix with the `LOCK_SH` flag, and the `LockFileEx` function on Windows.

Both of these seem supported and work properly on OS versions from the last few decades. [There is a quirk(https://utcc.utoronto.ca/~cks/space/blog/linux/FlockFcntlAndNFS) with `flock` on NFS if one client is accessing over NFS and another is directly accessing the local filesystem on the NFS server, but that's kind of a "don't do that" situation.

I haven't looked in detail at the implementation but the rough approach of making a .lock file in the directory, ensuring every client is referring go the same actual file by relying on atomic hardlinks into place (which does work reliably on NFS), and then using shared or exclusive locks of the same lock type (and not byte-range locks), seems sound to me.

I have a very slight worry about implementations that desugar `flock` into byte-range locks if there are no actual bytes in the file, i.e., the locked range is zero length. I am not sure if this is a real worry but we can just write one byte into the file to avoid the risk.

---

_Comment by @BurntSushi on 2025-09-17 12:19_

At least on the standard library side, support seems pretty broad: https://github.com/rust-lang/rust/blob/2ebb1263e3506412889410b567fa813ca3cb5c63/library/std/src/sys/fs/unix.rs#L1325-L1360

At least as broad as the regular `lock()` method.

---

_@zanieb approved on 2025-09-17 12:21_

---

_Comment by @zanieb on 2025-09-17 12:21_

I guess one other concern...

If the cache is read only, this will cause a regression?

---

_Comment by @BurntSushi on 2025-09-17 12:22_

If memory serves, `flock` and POSIX `fcntl` locks are process _global_. So the main footgun to be aware of here is that you can't use them as synchronization primitives across threads within the same process.

AFAIK, open file description locks on Linux support NFS and can be used as synchronization primitives across threads. But I think they are Linux-only unfortunately.

---

_Comment by @charliermarsh on 2025-09-17 14:14_

> If the cache is read only, this will cause a regression?

Like elsewhere, I'd suggest we `tracing::warn!` if we can't acquire it, but not fail.

---

_Comment by @konstin on 2025-09-17 14:16_

That's implemented :) <https://github.com/astral-sh/uv/pull/15888/files#diff-d0c8455b65232353aa60383cd8a80d99a8b31cf7cd76bf22c18d32de36bed34cR403-R409>

(I still have to test the read-only cache and handle the Windows CI failure before we can merge)

---

_Comment by @konstin on 2025-09-18 16:48_

It seems read-only caches already don't work, I filed a separate bug for it: https://github.com/astral-sh/uv/issues/15934

---

_Comment by @konstin on 2025-09-18 17:54_

I cannot reproduce the CI failure locally, any ideas what's happening? I could see something about the locked file not being deletable, but that should happen consistently, and it also doesn't match the error message.

---

_Comment by @zanieb on 2025-09-18 18:49_

I presume we attempt to delete the cache directory while it contains the locked file, which is okay on Unix but not allowed on Windows? (very naively)

---

_Comment by @zanieb on 2025-09-18 19:00_

re. CI vs locally... maybe because it's on a ReFS drive?

---

_Comment by @konstin on 2025-09-19 07:39_

I thought so too, but I'm using a ReFS drive on my laptop and it passes there.

---

_Comment by @konstin on 2025-09-19 08:21_

Removing the locked file last after the dropping the lock works.

---

_Merged by @konstin on 2025-09-19 08:21_

---

_Closed by @konstin on 2025-09-19 08:21_

---

_Branch deleted on 2025-09-19 08:21_

---

_Comment by @markuspi on 2025-11-07 14:45_

Hi @konstin considering this PR, is the note in the documentation regarding [cache safety](https://docs.astral.sh/uv/concepts/cache/#cache-safety) still relevant?

> Note that it's not safe to modify the uv cache (e.g., uv cache clean) while other uv commands are running, [...]

Specifically, i am interested in periodically running `uv cache prune`

---

_Comment by @konstin on 2025-11-07 15:19_

Currently, uv blocks, but we want to make some further adjustment that might weaken this a bit so I don't want to remove the warning. I don't recommend using `uv cache` operations on a timer, I'd only use them manually and when required.

---
