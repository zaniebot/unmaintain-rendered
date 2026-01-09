---
number: 11871
title: uv sync with find-links not locating wheels in the provided folder
type: issue
state: closed
author: stumpylog
labels:
  - bug
assignees: []
created_at: 2025-02-28T23:01:38Z
updated_at: 2025-03-02T15:58:35Z
url: https://github.com/astral-sh/uv/issues/11871
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync with find-links not locating wheels in the provided folder

---

_Issue opened by @stumpylog on 2025-02-28 23:01_

### Summary

Hopefully not a duplicate, but I wasn't able to locate anything existing in the tracker.

We pre-build some packages and `curl` them into the image ahead of time so CI doesn't need to build the same thing over and over.  We're now trying to transition to `uv` and finding it doesn't seem to find those local packages.

The current directory:
```bash
drwxr-xr-x 1 root root 4.0K Feb 28 22:52 .
drwxr-xr-x 1 root root 4.0K Feb 28 20:13 ..
-rw-r--r-- 1 root root 2.2M Feb 28 22:52 psycopg_c-3.2.4-cp312-cp312-linux_aarch64.whl
-rw-r--r-- 1 root root 2.3M Feb 28 22:52 psycopg_c-3.2.4-cp312-cp312-linux_x86_64.whl
-rw-rw-r-- 1 1000 1000 2.9K Feb 28 21:56 pyproject.toml
-rw-rw-r-- 1 1000 1000 493K Feb 28 21:46 uv.lock
-rw-r--r-- 1 root root 878K Feb 28 22:52 zxing_cpp-2.3.0-cp312-cp312-linux_aarch64.whl
-rw-r--r-- 1 root root 936K Feb 28 22:52 zxing_cpp-2.3.0-cp312-cp312-linux_x86_64.whl
```

Running this uv command:
`uv sync --verbose --frozen --no-dev --no-python-downloads --python-preference system --find-links ./ --no-build-package zxing-cpp --no-build-package psycopg-c`

fails with
`error: Distribution 'psycopg-c==3.2.4 @ registry+https://pypi.org/simple\' can't be installed because it is marked as '--no-build' but has no binary distribution`

I would expect uv to locate the wheel that already exists and install from it, instead of wanting to build the file again.  I have also tried moving the wheel files to a folder and providing that to `--find-links`, which did not change the behavior.

### Platform

Debian Bookworm (slim)

### Version

0.6.3

### Python version

3.12.9

---

_Label `bug` added by @stumpylog on 2025-02-28 23:01_

---

_Comment by @zanieb on 2025-02-28 23:04_

Interesting, thanks. I presume we're just not invalidating the lockfile here? Does it work if you explicitly lock first with `uv lock --find-links ...` or if you put `find-links` on your `pyproject.toml`? (just to try to rule out a few things)

If that has no effect, can you share a MRE in a Git repository or something so we can debug?

---

_Comment by @stumpylog on 2025-02-28 23:14_

I threw together a smaller version here: https://github.com/stumpylog/uv-find-links-mre.  Just a docker build should produce it.

Adding `uv lock --find-links ./` before the sync doesn't seem to have an effect.

---

_Comment by @charliermarsh on 2025-03-01 00:20_

Hmm, this seems expected though, right? You're locking against PyPI, but then trying to sync against a different package source (your local directory).

---

_Comment by @stumpylog on 2025-03-01 00:40_

It was unexpected to me.  At least the documentation seems to indicate wheel files here will be used. Nothing about where the lock source is.  This sort of thing is probably pretty common, maybe more for musl

---

_Comment by @charliermarsh on 2025-03-01 00:44_

If you look at the error message, the way to think of it is: uv is trying to install `psycopg-c==3.2.4 @ registry+https://pypi.org/simple`, i.e., it needs to find `psycopg-c` from PyPI. So it can't be content with installing an arbitrary built `psycopg-c` that exists in a local directory.

---

_Comment by @stumpylog on 2025-03-01 00:53_

If I'm understanding them, to do this everyone on the team would need to download the wheels to a folder, named the same and then I lock with find-links pointing there?

Is there any better solution?

---

_Comment by @charliermarsh on 2025-03-01 01:08_

Why do the wheels need to be downloaded? Can you not include the `--find-links` URL as a source when you lock? That would be my suggestion.

---

_Comment by @stumpylog on 2025-03-02 02:45_

I'd prefer to keep these downloaded only for the Docker image, rather than all users or developers.  Before I assume this is therefore intended, is there a reason why adding `uv lock --find-links ./` before the install wouldn't have updated the package source?

---

_Comment by @charliermarsh on 2025-03-02 02:58_

You'd probably need to `rm uv.lock` or use `--upgrade`? Otherwise, we won't invalidate the lock due to the addition of a source.

---

_Comment by @stumpylog on 2025-03-02 03:08_

Alright, I'll have to re-think this further.  Sadly it appears a Github release page doesn't work for finding links either, even if I wanted to go that route.

---

_Closed by @stumpylog on 2025-03-02 03:08_

---

_Comment by @stumpylog on 2025-03-02 15:25_

Sorry to keep asking questions, but why would this not attempt to pull in the wheels?  It seems to be valid, but does not get added to uv.lock in any fashion.

```toml
[tool.uv.sources]
psycopg-c = [
 { url = "https://github.com/paperless-ngx/builder/releases/download/psycopg-3.2.4/psycopg_c-3.2.4-cp312-cp312-linux_x86_64.whl", marker = "sys_platform == 'linux' and platform_machine == 'x86_64' and python_version == '3.12'" },
 { url = "https://github.com/paperless-ngx/builder/releases/download/psycopg-3.2.4/psycopg_c-3.2.4-cp312-cp312-linux_aarch64.whl", marker = "sys_platform == 'linux' and platform_machine == 'aarch64' and python_version == '3.12'" },
]
```

---

_Comment by @charliermarsh on 2025-03-02 15:28_

It looks fine but I think I'd need a more complete example to help you out. Is `pyscopg-c` directly as a dependency in the same file?

---

_Comment by @stumpylog on 2025-03-02 15:58_

It's not a direct dependency, it's pulled in by an extra.  The project I'm playing around with is https://github.com/stumpylog/uv-find-links-mre/blob/main/pyproject.toml

---
