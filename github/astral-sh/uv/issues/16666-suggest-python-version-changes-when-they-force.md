---
number: 16666
title: Suggest python version changes when they force you to select an sdist (and it fails to build)
type: issue
state: closed
author: Gankra
labels:
  - enhancement
assignees: []
created_at: 2025-11-10T13:53:15Z
updated_at: 2025-11-10T13:58:55Z
url: https://github.com/astral-sh/uv/issues/16666
synced_at: 2026-01-07T13:12:19-06:00
---

# Suggest python version changes when they force you to select an sdist (and it fails to build)

---

_Issue opened by @Gankra on 2025-11-10 13:53_

### Summary

The current release of pygame is 2.6.1 which has wheels up to 3.13, but not for 3.14. Since uv defaults to 3.14 when you setup a project, this makes the pygame hello-world experience for uv "you get SDL.h is missing errors and you don't know what to do".

It would be great if we could detect this situation when the sdist build fails (and/or if you pass --no-build).

### Example

Here are steps to reproduce with `--no-build`:

```
$ uv python pin 3.14
$ uv add pygame==2.6.1 --no-build

error: Distribution `pygame==2.6.1 @ registry+https://pypi.org/simple` can't be installed because it is marked as `--no-build` but has no binary distribution

$ uv python pin 3.13
$ uv add pygame==2.6.1 --no-build

Using CPython 3.13.5
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 2 packages in 3ms
Installed 1 package in 12ms
 + pygame==2.6.1
```

(Seen on macOS, with `uv 0.9.7 (0adb44480 2025-10-30)`, but the general problem is cross-platform)

---

_Label `enhancement` added by @Gankra on 2025-11-10 13:53_

---

_Comment by @Gankra on 2025-11-10 13:58_

Found the dupe!

---

_Closed by @Gankra on 2025-11-10 13:58_

---
