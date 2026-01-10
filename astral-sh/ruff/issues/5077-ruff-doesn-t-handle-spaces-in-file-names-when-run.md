---
number: 5077
title: "(🐞) Ruff doesn't handle spaces in file names when run through python"
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
  - cli
assignees: []
created_at: 2023-06-14T05:38:35Z
updated_at: 2023-06-15T18:57:02Z
url: https://github.com/astral-sh/ruff/issues/5077
synced_at: 2026-01-10T01:22:44Z
---

# (🐞) Ruff doesn't handle spaces in file names when run through python

---

_Issue opened by @KotlinIsland on 2023-06-14 05:38_

```
> ruff "a b.py"
> python -m ruff "a b.py"
error: Failed to lint a: The system cannot find the file specified. (os error 2)
error: Failed to lint b.py: The system cannot find the file specified. (os error 2)
a:1:1: E902 The system cannot find the file specified. (os error 2)
b.py:1:1: E902 The system cannot find the file specified. (os error 2)
Found 2 errors.
```

Windows 10
powershell 7.4 / 5.1 and CMD (and I think invoking it programmatically as well)
ruff 0.0.272
python 3.11.2

---

_Assigned to @konstin by @konstin on 2023-06-14 13:13_

---

_Label `bug` added by @charliermarsh on 2023-06-14 13:28_

---

_Label `cli` added by @charliermarsh on 2023-06-14 13:28_

---

_Comment by @zanieb on 2023-06-14 16:02_

@KotlinIsland / @konstin isn't this just because double quotes are used? `python -m ruff 'a b.py'` works as intended (on macOS), as does `python -m ruff a\ b.py`.

edit: also cannot reproduce with double quotes — I thought I had but can't now. Maybe Windows specific?

---

_Comment by @konstin on 2023-06-14 16:10_

I can't reproduce this with PowerShell 7.3.4 on ubuntu 22.04 (`python -m ruff "a b.py"` works), if it doesn't resolve itself until then i'll boot into windows and check tomorrow

---

_Comment by @KotlinIsland on 2023-06-14 21:37_

using an escape character for the space showed the same result

Works
```
ruff a` b.py
```

Doesn't work
```
python -m ruff a` b.py
```

---

_Referenced in [astral-sh/ruff#5115](../../astral-sh/ruff/pulls/5115.md) on 2023-06-15 07:48_

---

_Comment by @konstin on 2023-06-15 07:49_

I could reproduce this when using windows instead of linux and fixed this in #5115

---

_Closed by @konstin on 2023-06-15 18:57_

---
