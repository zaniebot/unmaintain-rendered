```yaml
number: 16391
title: Install locally built python version.
type: issue
state: open
author: TheDreadedAndy
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-10-21T16:59:17Z
updated_at: 2025-10-22T17:49:19Z
url: https://github.com/astral-sh/uv/issues/16391
synced_at: 2026-01-12T16:02:30Z
```

# Install locally built python version.

---

_@TheDreadedAndy_

### Summary

I'm trying to have uv manage a python version that I've built locally using python-build-standalone. However, as far as I can tell there is no way to do this.

What I'd like to be able to do is something like:
```shell
uv python install path/to/cpython.tar.zst
```

### Example

This would be useful for, e.g., using `uv` in an offline environment where there is no locally hosted mirror.

---

_Label `enhancement` added by @TheDreadedAndy on 2025-10-21 16:59_

---

_Comment by @konstin on 2025-10-21 17:55_

Why are you using a locally build version instead of the ones provided in the python build standalone release? (You're the first person to bring this up, so I gotta ask)

---

_Label `wish` added by @zanieb on 2025-10-21 17:58_

---

_Comment by @lemming622 on 2025-10-22 14:42_

I'm in an air-gapped system and this is a thing I'm also interested in.

---

_Comment by @TheDreadedAndy on 2025-10-22 15:20_

> Why are you using a locally build version instead of the ones provided in the python build standalone release? (You're the first person to bring this up, so I gotta ask)

I had a need to compile python against a TCL where threading was disabled.

Regardless, I was able to solve this by just extracting the `python/install` folder in the build and pointing `uv` to that when I made the venv, which works sufficiently well for my use case.

---

_Comment by @zanieb on 2025-10-22 17:48_

@lemming622 you should just set `UV_PYTHON_CACHE` and then transport that directory to your air-gapped system.

---

_Comment by @zanieb on 2025-10-22 17:49_

> Regardless, I was able to solve this by just extracting the python/install folder in the build and pointing uv to that when I made the venv, which works sufficiently well for my use case.

I think this will be our recommendation here. I don't think being able to unpack an arbitrary distribution via uv provides enough value to justify the complexity.

---
