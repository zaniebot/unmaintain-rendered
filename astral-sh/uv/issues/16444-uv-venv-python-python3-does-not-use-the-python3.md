---
number: 16444
title: "`uv venv --python python3` does not use the `python3` executable"
type: issue
state: open
author: AndydeCleyre
labels:
  - question
assignees: []
created_at: 2025-10-24T23:51:00Z
updated_at: 2025-10-25T00:41:50Z
url: https://github.com/astral-sh/uv/issues/16444
synced_at: 2026-01-10T01:26:06Z
---

# `uv venv --python python3` does not use the `python3` executable

---

_Issue opened by @AndydeCleyre on 2025-10-24 23:51_

### Summary

Zsh:

```console
$ uv -V; python -V; python3 -V
uv 0.9.5
Python 3.14.0
Python 3.14.0
$ uv venv --python python3 --verbose badvenv
DEBUG uv 0.9.5
DEBUG Acquired shared lock for `/home/andy/.cache/uv`
DEBUG Using Python request `3` from explicit request
DEBUG Searching for Python 3 in managed installations or search path
DEBUG Searching for managed installations at `/home/andy/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.13-linux-x86_64-gnu`
DEBUG Found `cpython-3.11.13-linux-x86_64-gnu` at `/home/andy/.local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/bin/python3.11` (managed installations)
Using CPython 3.11.13
Creating virtual environment at: badvenv
DEBUG Assessing Python executable as base candidate: /home/andy/.local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/bin/python3.11
DEBUG Using base executable for virtual environment: /home/andy/.local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/bin/python3.11
Activate with: source badvenv/bin/activate
DEBUG Released lock at `/home/andy/.cache/uv/.lock`
$ uv venv --python python --verbose goodvenv
DEBUG uv 0.9.5
DEBUG Acquired shared lock for `/home/andy/.cache/uv`
DEBUG Using Python request `python` from explicit request
DEBUG Searching for Python interpreter with executable name `python`
DEBUG Found `cpython-3.14.0-linux-x86_64-gnu` at `/home/andy/.local/share/mise/installs/python/3.14.0/bin/python` (search path)
Using CPython 3.14.0 interpreter at: /home/andy/.local/share/mise/installs/python/3.14.0/bin/python
Creating virtual environment at: goodvenv
DEBUG Assessing Python executable as base candidate: /home/andy/.local/share/mise/installs/python/3.14.0/bin/python
DEBUG Using base executable for virtual environment: /home/andy/.local/share/mise/installs/python/3.14.0/bin/python
Activate with: source goodvenv/bin/activate
DEBUG Released lock at `/home/andy/.cache/uv/.lock`
```

Looking at this debug output, it seems `--python` may not be what I had long thought it was -- the interpreter executable, to be found in the current `PATH`. Is that by design? I strikes me as a bug when `python3 -V` reports `3.14.0` and `uv venv --python python3` uses `3.11.13`.

It seems it *is* what I thought when and only when the value is literally `python`, but something else when it's something different (`python3`).

I see now in the docs (but not the `--help` output) that some expected values are `3.12.0` and `pypy@3.8`. Does that mean there's no way to have uv use "whatever `python3` refers to" if it happens to differ from what `python` refers to?

I would guess that since it's not a straight number (`3`) or `@`-form (`python@3`) that it would be in the same class of value as `python`, which is the interpreter executable on the `PATH`.

### Platform

Linux 6.17.2-zen1-1-zen x86_64 GNU/Linux

### Version

uv 0.9.5

### Python version

Python 3.14.0

---

_Label `bug` added by @AndydeCleyre on 2025-10-24 23:51_

---

_Comment by @zanieb on 2025-10-25 00:18_

Yes, `--python` does not treat `python3` as a request for that executable name, but rather any Python 3 interpreter. I understand this can be surprising, but there are various nuanced reasons it's that way and I'm pretty hesitant to change it.

`--python python` is indeed treated as a request for that executable name, but that's just our fallback for any unrecognized request, e.g., `--python foobar` also does that.

I'd recommend doing `--python $(which python3)` if you want the behavior you're describing.

---

_Label `bug` removed by @zanieb on 2025-10-25 00:19_

---

_Label `question` added by @zanieb on 2025-10-25 00:19_

---

_Comment by @AndydeCleyre on 2025-10-25 00:41_

Ok, thanks. I do think this is surprising enough to warrant a hint in the `--help` description.

---
