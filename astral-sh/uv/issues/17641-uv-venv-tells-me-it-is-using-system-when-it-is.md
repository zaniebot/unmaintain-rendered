```yaml
number: 17641
title: uv venv tells me it is using system when it is really not
type: issue
state: closed
author: ldng
labels:
  - question
assignees: []
created_at: 2026-01-21T16:03:46Z
updated_at: 2026-01-21T16:13:50Z
url: https://github.com/astral-sh/uv/issues/17641
synced_at: 2026-01-21T17:03:56Z
```

# uv venv tells me it is using system when it is really not

---

_@ldng_

### Summary

```
$ uv venv --system
warning: The `--system` flag has no effect, a system Python interpreter is always used in `uv venv`
Using CPython 3.14.0+freethreaded
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
$ python --version
Python 3.14.2
```

So, `uv`, it looks like you're lying to me here ...
A freethreaded 3.14.0 is being used to create the virtualenv when my system has a 3.14.2 with GIL.

### Platform

Linux 6.18.6-2-cachyos x86_64 GNU/Linux

### Version

uv 0.9.26 (ee4f00362 2026-01-15)

### Python version

Python 3.14.0

---

_Label `bug` added by @ldng on 2026-01-21 16:03_

---

_Comment by @ldng on 2026-01-21 16:05_

If more info is needed, happy to oblige :-)

---

_Comment by @zanieb on 2026-01-21 16:07_

Here, the `--system` flag means "not a virtual environment interpreter". In the same sense that `uv pip install --system` skips virtual environments.

It sounds like you're looking for `--no-managed-python` which is our replacement for `--python-preference system` due to the confusion from those overlapping terms. 

---

_Label `bug` removed by @zanieb on 2026-01-21 16:07_

---

_Label `question` added by @zanieb on 2026-01-21 16:07_

---

_Comment by @ldng on 2026-01-21 16:13_

Indeed I just find out that `--no-managed-python` is what I'm actually looking for. Thanks for the quick answer.
Closing (although the message could be changed to dissipate any ambiguity)

---

_Closed by @ldng on 2026-01-21 16:13_

---
