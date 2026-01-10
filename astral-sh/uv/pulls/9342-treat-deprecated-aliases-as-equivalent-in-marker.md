```yaml
number: 9342
title: Treat deprecated aliases as equivalent in marker algebra
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/lower-i
created_at: 2024-11-22T00:32:05Z
updated_at: 2024-11-26T14:27:25Z
url: https://github.com/astral-sh/uv/pull/9342
synced_at: 2026-01-10T12:00:00Z
```

# Treat deprecated aliases as equivalent in marker algebra

---

_Pull request opened by @charliermarsh on 2024-11-22 00:32_

## Summary

This PR modifies our lowered representation such that any deprecated aliases are treated as "the same" marker in the algebra.

So, for example, we now recognize that this is impossible, despite the marker names being different:

```
typing-extensions ; platform.python_implementation == 'CPython' and python_implementation != 'CPython'
```

Similarly, we now recognize that this is just `sys_platform == 'win32'`, despite the presence of both markers:

```
anyio ; sys_platform == 'win32' and sys.platform == 'win32'
```


---

_Label `bug` added by @charliermarsh on 2024-11-22 00:32_

---

_@konstin approved on 2024-11-26 10:29_

---

_Merged by @charliermarsh on 2024-11-26 14:27_

---

_Closed by @charliermarsh on 2024-11-26 14:27_

---

_Branch deleted on 2024-11-26 14:27_

---
