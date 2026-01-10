```yaml
number: 2171
title: Run Windows against Python 3.13
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/win
created_at: 2024-03-04T19:33:34Z
updated_at: 2024-03-04T21:49:18Z
url: https://github.com/astral-sh/uv/pull/2171
synced_at: 2026-01-10T14:54:43Z
```

# Run Windows against Python 3.13

---

_Pull request opened by @charliermarsh on 2024-03-04 19:33_

## Summary

In Python 3.13, at least in the current builds, there's no `python.exe`, but there is `venvlauncher.exe`.

I've asked here about whether it's intended: https://discuss.python.org/t/when-should-venv-scripts-nt-python-exe-be-present/47620. But there's at least some evidence in CPython [here](https://github.com/python/cpython/blob/d457345bbc6414db0443819290b04a9a4333313d/Lib/venv/__init__.py#L270) that we should fall back to these, and the tests pass.

Closes https://github.com/astral-sh/uv/issues/1636.

---

_Label `bug` added by @charliermarsh on 2024-03-04 21:45_

---

_Label `windows` added by @charliermarsh on 2024-03-04 21:45_

---

_Marked ready for review by @charliermarsh on 2024-03-04 21:45_

---

_Merged by @charliermarsh on 2024-03-04 21:49_

---

_Closed by @charliermarsh on 2024-03-04 21:49_

---

_Branch deleted on 2024-03-04 21:49_

---
