```yaml
number: 15707
title: "\"uv python list\" or \"uv run python\" open online Python help"
type: issue
state: closed
author: dalito
labels:
  - bug
assignees: []
created_at: 2025-09-06T12:39:42Z
updated_at: 2025-09-06T16:52:01Z
url: https://github.com/astral-sh/uv/issues/15707
synced_at: 2026-01-10T03:23:54Z
```

# "uv python list" or "uv run python" open online Python help

---

_Issue opened by @dalito on 2025-09-06 12:39_

### Summary

With the relatively new Python Install Manager (I have v25.0.189.0) running `uv python list` first opens the webpage https://docs.python.org/dev/using/windows.html and then lists the python versions. Also other commands show this behavior, e.g. `uv run python --version` (or `uv run py --version`). After opening the web page the commands succeed as normal.

I guess this comes from running `python` as command internally. The web page is also opened if I execute `python` in a cmd or powershell window. The new Python Install Manager does no longer map `python` to a default Python like the Python launcher did.

### Platform

Windows 11, MSYS_NT-10.0-26100 3.5.7-882031da.x86_64 x86_64 Msys

### Version

0.8.15 (8473ecba1 2025-09-03)

### Python version

Python 3.12.10, Python 3.13.7

---

_Label `bug` added by @dalito on 2025-09-06 12:39_

---

_Renamed from ""uv python list" opens online Python help" to ""uv python list" or "uv run python" open online Python help" by @dalito on 2025-09-06 12:40_

---

_Comment by @zanieb on 2025-09-06 13:43_

That's a frustrating quality. How did you install that? We'll either need to detect it's that Python or ask that they add a way to opt-out of that behavior so we query the interpreter.

---

_Comment by @dalito on 2025-09-06 15:10_

I downloaded it from https://www.python.org/downloads/windows/ two weeks ago. 

Now I see that there is a newer version of the Install Manager but I don't have it yet although it is said to update automatically. Maybe https://github.com/python/pymanager/pull/157/files fixed the problem - I'll test...

---

_Comment by @dalito on 2025-09-06 16:11_

Updating Python install manager to 25.0b14 fixed the issue. Great!

---

_Closed by @dalito on 2025-09-06 16:52_

---
