---
number: 14085
title: "`uv init --script` doesn't respect an existing shebang"
type: issue
state: closed
author: oconnor663
labels:
  - bug
assignees: []
created_at: 2025-06-16T19:17:37Z
updated_at: 2025-06-19T21:47:23Z
url: https://github.com/astral-sh/uv/issues/14085
synced_at: 2026-01-10T01:25:42Z
---

# `uv init --script` doesn't respect an existing shebang

---

_Issue opened by @oconnor663 on 2025-06-16 19:17_

### Summary

```
$ cat myscript.py
#! /usr/bin/env python3

print("my existing script")

$ uv init --script myscript.py
Initialized script at `myscript.py`

$ cat myscript.py
# /// script
# requires-python = ">=3.13"
# dependencies = []
# ///

#! /usr/bin/env python3

print("my existing script")
```

### Platform

linux

### Version

uv 0.7.13 (62ed17b23 2025-06-12)

### Python version

_No response_

---

_Label `bug` added by @oconnor663 on 2025-06-16 19:17_

---

_Comment by @zanieb on 2025-06-16 23:03_

This is pretty weird, e.g., `./myscript.py` would fail because `python myscript.py` doesn't read PEP 723 dependency metadata. I'm not sure we should allow this? It seems very hard to determine if the shebang will respect the dependency metadata nicely.

---

_Comment by @oconnor663 on 2025-06-16 23:43_

The use case that led me to try this is that I had a script that expected its dependencies to "just be there", either globally or in the active venv. Adding inline script metadata (below the shebang) means that the previous workflow still works, but `uv run myscript.py` also works if you want. Is that a weird adoption path?

---

_Comment by @zanieb on 2025-06-17 01:29_

I worry about confusion. We could warn? but we might have false positives? Anyway, seems more correct to respect it than not. We can open something else to track warning when shebangs are present.

---

_Referenced in [astral-sh/uv#14141](../../astral-sh/uv/pulls/14141.md) on 2025-06-18 22:24_

---

_Closed by @oconnor663 on 2025-06-19 21:47_

---
