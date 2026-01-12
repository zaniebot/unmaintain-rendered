```yaml
number: 13020
title: Incorrect pip compile for git subdirectory dependency
type: issue
state: closed
author: bnaul
labels:
  - bug
assignees: []
created_at: 2025-04-21T18:52:38Z
updated_at: 2025-06-26T19:48:13Z
url: https://github.com/astral-sh/uv/issues/13020
synced_at: 2026-01-12T16:01:17Z
```

# Incorrect pip compile for git subdirectory dependency

---

_@bnaul_

### Summary

Example `requirements.in`:
```
git+https://github.com/PrefectHQ/prefect.git#subdirectory=src/integrations/prefect-dask
```
Compiling gives a weird-looking relative reference for `prefect` that does not install correctly
```
➜ uv pip compile requirements.in | grep '^prefect'
Resolved 105 packages in 193ms
prefect @ ../../../@562b964535b46f586b9b12808462956ed2f90c5a
prefect-dask @ git+https://github.com/PrefectHQ/prefect.git@562b964535b46f586b9b12808462956ed2f90c5a#subdirectory=src/integrations/prefect-dask
```
The same thing happens if you declare `prefect` in `requirements.in`, but adding `prefect` as a `git+` dependency does work.

### Platform

MacOS 15 ARM64

### Version

uv 0.6.14 (Homebrew 2025-04-09)

### Python version

3.13.2, 3.10.16

---

_Label `bug` added by @bnaul on 2025-04-21 18:52_

---

_Comment by @charliermarsh on 2025-06-23 02:02_

Thanks, this is definitely a bug. I believe that's the path to the `prefect` root _within our cache_. We fix these up in a variety of places, but we might be missing one? Or there's something else going on. @konstin has some context here too, I think.

---

_Comment by @konstin on 2025-06-26 14:20_

The problem is that we use the incorrect `given` value for the path-in-git case, and the given value is what's displayed: https://github.com/astral-sh/uv/blob/4b348512c270d4fc45d4c5ab55df0bae4e2d93cd/crates/uv-distribution/src/metadata/lowering.rs#L691-L713

---

_Comment by @charliermarsh on 2025-06-26 14:23_

Oh interesting. I wonder why that hasn't affected use elsewhere. Honestly, we might want to drop `given` entirely for sources? `given` mostly existed to help with preserving environment variables in `requirements.txt` files.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-26 15:37_

---

_Comment by @charliermarsh on 2025-06-26 15:37_

I can take a look from here @konstin.

---

_Closed by @charliermarsh on 2025-06-26 19:48_

---
