```yaml
number: 7281
title: "`uv run` supports python package"
type: pull_request
state: merged
author: j178
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: run-package
created_at: 2024-09-11T05:39:31Z
updated_at: 2024-09-11T21:57:47Z
url: https://github.com/astral-sh/uv/pull/7281
synced_at: 2026-01-10T12:53:44Z
```

# `uv run` supports python package

---

_Pull request opened by @j178 on 2024-09-11 05:39_

## Summary

Allow `uv run ./package` runs a Python package with a `__main__.py` script.

Resolves #7275


---

_Renamed from "`uv run` support a python package" to "`uv run` supports python package" by @j178 on 2024-09-11 05:41_

---

_@zanieb approved on 2024-09-11 13:17_

Cool, thanks!

---

_Merged by @zanieb on 2024-09-11 13:18_

---

_Closed by @zanieb on 2024-09-11 13:18_

---

_Label `enhancement` added by @zanieb on 2024-09-11 13:18_

---

_Label `cli` added by @zanieb on 2024-09-11 13:18_

---

_Branch deleted on 2024-09-11 13:23_

---

_Comment by @gotmax23 on 2024-09-11 17:55_

This seems strange. Why not add `-m` as was suggested in https://github.com/astral-sh/uv/issues/6638#issuecomment-2310076398?

---

_Comment by @j178 on 2024-09-11 21:57_

They serve different purposes. This PR (along with #7289) aims to make `uv run` works like a python launcher. This PR allows `uv run` to execute a directory containing a `__main__.py` file. Refer to https://docs.python.org/3/using/cmdline.html#interface-options for more details. On the other hand, `python -m` is used to run a module from the sys.path, and it's not equivalent to running a directory containing a `__main__.py` file. 

---
