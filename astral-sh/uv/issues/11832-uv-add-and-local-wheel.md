```yaml
number: 11832
title: uv add and local wheel
type: issue
state: closed
author: traits
labels:
  - question
assignees: []
created_at: 2025-02-27T11:47:20Z
updated_at: 2025-09-02T21:29:00Z
url: https://github.com/astral-sh/uv/issues/11832
synced_at: 2026-01-12T16:00:47Z
```

# uv add and local wheel

---

_@traits_

### Summary

When adding a local wheel on Windows with absolute path  (`uv add v:/wheels/triton-3.2.0-cp312-cp312-win_amd64.whl`) `project.toml` is rewritten in using a correct but relative path:

```
[tool.uv.sources]
triton = { path = "../../../wheels/triton-3.2.0-cp312-cp312-win_amd64.whl" }
```

This is not always intended, e.g. if the project is moved to another directory. uv should probably retain the absolute path if one has been specified (or providing an option to change whatever default behavior. I hope I didn't miss any).

### Platform

Windows 11

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

Python 3.12.9

---

_Label `bug` added by @traits on 2025-02-27 11:47_

---

_Comment by @charliermarsh on 2025-02-28 00:18_

This is by design: everything in the lockfile is resolved relative to the project root. I get that an absolute path would be more convenient in the case you described above, but absolute paths are extremely non-portable, and lockfiles are intended to be committed.

---

_Label `bug` removed by @charliermarsh on 2025-02-28 00:18_

---

_Label `question` added by @charliermarsh on 2025-02-28 00:18_

---

_Closed by @charliermarsh on 2025-03-01 00:30_

---

_Comment by @vadimkantorov on 2025-09-02 21:28_

This is a useful feature, in cases with networked drives (e.g. we have an existing pip wheel cache on a networked drive) in lab enviornments, in this case absolute paths are actually more portable

---
