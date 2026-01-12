```yaml
number: 1975
title: Remove current directory from PATH in PEP 517 hooks
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cwd
created_at: 2024-02-26T01:02:29Z
updated_at: 2024-02-26T01:14:15Z
url: https://github.com/astral-sh/uv/pull/1975
synced_at: 2026-01-12T16:04:49Z
```

# Remove current directory from PATH in PEP 517 hooks

---

_@charliermarsh_

## Summary

When you invoke `python -c`, an empty string is prepended to `sys.path`, which allows loading modules in the current directory (https://docs.python.org/3/using/cmdline.html#cmdoption-P). However, in PEP 517 builds, the current directory should _not_ be part of the path. There's a flag we can use to disable this behavior (`-P`), but it's only available in Python 3.11 and later, so instead, I'm doing something similar to pip's `__main__.py`, which avoids this for `python -m pip` invocations.

Closes https://github.com/astral-sh/uv/issues/1972.


---

_Label `bug` added by @charliermarsh on 2024-02-26 01:02_

---

_Merged by @charliermarsh on 2024-02-26 01:14_

---

_Closed by @charliermarsh on 2024-02-26 01:14_

---

_Branch deleted on 2024-02-26 01:14_

---
