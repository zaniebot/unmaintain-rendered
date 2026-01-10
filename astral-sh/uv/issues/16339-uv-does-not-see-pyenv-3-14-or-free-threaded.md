---
number: 16339
title: "`uv` does not see Pyenv 3.14 or free-threaded Python installs"
type: issue
state: closed
author: davidhewitt
labels:
  - question
assignees: []
created_at: 2025-10-17T09:44:13Z
updated_at: 2025-10-20T12:20:39Z
url: https://github.com/astral-sh/uv/issues/16339
synced_at: 2026-01-10T01:26:05Z
---

# `uv` does not see Pyenv 3.14 or free-threaded Python installs

---

_Issue opened by @davidhewitt on 2025-10-17 09:44_

### Summary

Install Python 3.13, 3.13t, 3.14 and 3.14t with `pyenv`:

```bash
pyenv install 3.13 --keep
pyenv install 3.13t --keep
pyenv install 3.14 --keep
pyenv install 3.14t --keep

pyenv versions

uv python list
```

pyenv output:

```
* 3.13.9 (set by /home/david/.pyenv/version)
  3.13.9t
  3.14.0
  3.14.0t
```

uv output:

```
cpython-3.14.0-linux-x86_64-gnu                   <download available>  
cpython-3.14.0+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.13.9-linux-x86_64-gnu                   /home/david/.pyenv/shims/python3.13
cpython-3.13.9-linux-x86_64-gnu                   /home/david/.pyenv/shims/python3
cpython-3.13.9-linux-x86_64-gnu                   /home/david/.pyenv/shims/python
cpython-3.13.9-linux-x86_64-gnu                   <download available>
cpython-3.13.9+freethreaded-linux-x86_64-gnu      <download available>
```

`uv` sees the 3.13.9 default build, but none of the 3.14 or free-threaded ones.



### Platform

Ubuntu 24.04 x86_64

### Version

uv 0.9.3

### Python version

_No response_

---

_Label `bug` added by @davidhewitt on 2025-10-17 09:44_

---

_Comment by @konstin on 2025-10-20 09:36_

Are those installation added to `PATH`? uv checks for all `python` in `PATH`, but not in pyenv's specific structure.

---

_Comment by @davidhewitt on 2025-10-20 12:05_

Ah, no. I had somehow observed this working well enough in the past that I thought `uv` was doing something smart with pyenv. I guess close this issue if it's expected behaviour 👍 

---

_Label `bug` removed by @konstin on 2025-10-20 12:19_

---

_Label `question` added by @konstin on 2025-10-20 12:19_

---

_Comment by @konstin on 2025-10-20 12:20_

Yep, we're not inspecting pyenv installations separately, we rely on users adding those that we should find to `PATH`.

---

_Closed by @konstin on 2025-10-20 12:20_

---
