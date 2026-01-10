---
number: 6147
title: gracefully handle missing files/directories in cache storage
type: issue
state: closed
author: paveldikov
labels:
  - bug
assignees: []
created_at: 2024-08-16T15:18:11Z
updated_at: 2025-03-03T16:04:45Z
url: https://github.com/astral-sh/uv/issues/6147
synced_at: 2026-01-10T01:23:56Z
---

# gracefully handle missing files/directories in cache storage

---

_Issue opened by @paveldikov on 2024-08-16 15:18_

Not sure if this is a common pattern, but we like to store the cache directory on `/var/tmp` as the usual home directory may be on a network share (and extremely slow I/O as a result).

This works great for us, but it also happens that `/var/tmp` is affected by a periodic cleanup process that deletes old files/directories. Once this happens, we get a bunch of error messages along the lines of:

```
  Caused by: Failed to install: packaging-24.1-py3-none-any.http.whl (packaging==24.1)
  Caused by: failed to read directory `/var/tmp/pavel/.cache/uv/archive-v0/REDACTED`
  Caused by: No such file or directory (os error 2)
```

`rm -rf`'ing the cache dir (or `uv cache clean` -- but not `uv cache prune`) seems to work around this problem fine, but it's quite a severe thing to do.

---

_Comment by @charliermarsh on 2024-08-16 15:21_

Agree, I think we should be robust to this. I don't know if we can be robust to `/var/tmp` being cleaned up _while_ uv is running though.

---

_Label `bug` added by @charliermarsh on 2024-08-16 15:21_

---

_Comment by @paveldikov on 2024-08-16 15:26_

I think catching ENOENT and friends (at arbitrary depth?) and re-caching the distribution from scratch should be robust enough? I am trying to think of the various types of race conditions in there, but assuming that the hardlinking/copying into venv comes right at the end, this should be reasonably complete.

(symlink mode will not be helped by this, but that is probably fine?)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-20 20:41_

---

_Referenced in [astral-sh/uv#6284](../../astral-sh/uv/pulls/6284.md) on 2024-08-20 23:39_

---

_Closed by @charliermarsh on 2024-08-20 23:56_

---

_Comment by @paveldikov on 2025-01-03 14:12_

Heads up, I am seeing possible regressions on this one. I wonder if it may be worth adding an integration test for this scenario:
1. do a `uv pip install` of some skeleton package (iirc a bunch of these already exist as test fixtures)
2. then vandalise the cache by removing one of the files
3. wipe the venv (recreate as empty)
4. `uv pip install` it again. It should succeed and the removed file should be present in the installation.

---

_Comment by @charliermarsh on 2025-01-03 16:13_

Sounds good to me. Feel free to file an issue for any regression that you've seen.

---

_Comment by @charliermarsh on 2025-01-03 16:27_

I'll add a few tests.

---

_Comment by @paveldikov on 2025-01-03 18:05_

Ok, I tried to repro by hand by chaos-monkeying with my `archive-v0` directory and I found two things so far:
* if I remove a file from the package itself e.g. (`pytest/__init__.py`) and then install, I get a false-positive. Install is outwardly successful, but missing `__init__.py` i.e. it's broken.
* if I remove stuff from `dist-info` e.g. `RECORD`, the whole installation fails non-gracefully, and I must use `--reinstall` to remedy this. Not even `uv cache prune` fixes this.

---

_Comment by @charliermarsh on 2025-01-03 18:44_

I don’t know that we should necessarily aim to support these scenarios. Scenarios in which users modify the cache seem out of scope, and should be fixed by uv cache clean (as opposed to prune). What we _do_ want to be robust to are, e.g., changes in the cache schema, or any states that can result from running uv commands.

---

_Comment by @paveldikov on 2025-01-03 18:56_

It's a synthetic reproduction of a real-life scenario, whereby a daemon process (e.g. `tmpwatch`) prunes files from a cache directory based on some coarse-grained criterion such as mtime/ctime.

I would agree that _mutating_ files would be out of scope, though.

---

_Comment by @paveldikov on 2025-03-03 16:04_

Ok, I had a deeper dive with the code and the problem is complicated by `tmpwatch`'s behaviour in that it will delete files but not directories.  It is the 'deleting a given archive in isolation' that will trigger this problem -- I have a test case written for it which I can push out.

Applying the same treatment to the _whole_ cache dir appears to sidestep the issue, presumably because some top-level lookup table is also wiped out -- and therefore everything registers as a cache miss. However, IRL this is not likely to happen as the top-level lookup table will usually have a much more recent `mtime` because it is the common denominator in all cache operations.

So I am now stuck trying to do either of:
1. Prevent the cache lookup from picking up 'skeleton' archive entries -- this is proving difficult because it appears that the cache mechanics are type-agnostic and unaware of archive semantics/`dist-info`. Also I'd be worried about performance impact if I was to add elements of look-before-you-leap
2. Introduce a mechanism whereby we can safely backtrack on `ENOENT` -- this is proving difficult because the call stack is yay-deep and this would be extremely invasive in terms of function signatures 😰

(Unfortunately in my organisation I can't quite sidestep `tmpwatch`, so that's sadly not an option)


---

_Referenced in [astral-sh/uv#16761](../../astral-sh/uv/issues/16761.md) on 2025-11-17 15:25_

---
