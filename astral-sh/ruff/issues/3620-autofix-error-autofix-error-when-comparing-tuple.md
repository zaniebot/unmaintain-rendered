```yaml
number: 3620
title: "[Autofix error] Autofix error when comparing tuple"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-20T08:17:35Z
updated_at: 2023-03-22T16:32:02Z
url: https://github.com/astral-sh/ruff/issues/3620
synced_at: 2026-01-10T11:09:46Z
```

# [Autofix error] Autofix error when comparing tuple

---

_Issue opened by @qarmin on 2023-03-20 08:17_

Ruff  a45753f462c7a53afd7f942ab3c6f9af3871bf1f

```
def _detect_pathlib_path(p):
	if(3,4)<=sys.version_info:
	    pass
```
with 
```
ruff file.py --fix
```

```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Desktop/RunEveryCommand/Ruff/Broken/45224decoder (20th copy)0.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```


---

_Label `bug` added by @charliermarsh on 2023-03-20 15:11_

---

_Comment by @kyoto7250 on 2023-03-22 14:45_

I want to make a PR for this issue.
I think this bug caused by E231.

---

_Comment by @charliermarsh on 2023-03-22 15:33_

That may be one part of the problem, though the other (I think) is that we end up inverting to:

```py
def _detect_pathlib_path(p):
	ifsys.version_info>=(3,4):
	    pass
```

Which is a syntax error.

---

_Closed by @charliermarsh on 2023-03-22 16:32_

---
