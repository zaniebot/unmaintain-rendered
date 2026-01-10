```yaml
number: 8914
title: Enable uv to replace and delete itself on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/self
created_at: 2024-11-08T02:41:35Z
updated_at: 2024-11-08T15:53:20Z
url: https://github.com/astral-sh/uv/pull/8914
synced_at: 2026-01-10T12:00:00Z
```

# Enable uv to replace and delete itself on Windows

---

_Pull request opened by @charliermarsh on 2024-11-08 02:41_

## Summary

On Windows, we can't delete the currently-running executable -- at least, not trivially. But the [`self_replace`](https://docs.rs/self-replace/latest/self_replace/) crate can help us here.

Closes https://github.com/astral-sh/uv/issues/1368.

Closes https://github.com/astral-sh/uv/issues/4980.

## Test Plan

On my Windows machine:

- `maturin build`
- `python -m venv .venv`
- `.venv/Scripts/activate`
- `pip install /path/to/uv.whl`
- `uv pip install /path/to/uv.whl`
- `uv pip uninstall uv`


---

_Label `bug` added by @charliermarsh on 2024-11-08 02:43_

---

_Label `windows` added by @charliermarsh on 2024-11-08 02:43_

---

_Merged by @charliermarsh on 2024-11-08 02:57_

---

_Closed by @charliermarsh on 2024-11-08 02:57_

---

_Branch deleted on 2024-11-08 02:57_

---

_Comment by @notatallshaw on 2024-11-08 15:53_

Wow, this is amazing, it should probably looked to see if `pip` (or `distlib`) can implement the logic of `self_replace`, because this remains an issue on Windows if you call `pip` instead of `python -m pip`.

---
