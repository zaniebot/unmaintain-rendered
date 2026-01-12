```yaml
number: 17087
title: "`--project <path>` does not fail if `<path>` does not exist"
type: issue
state: open
author: zanieb
labels:
  - bug
  - breaking
assignees: []
created_at: 2025-12-11T15:23:02Z
updated_at: 2025-12-11T15:23:02Z
url: https://github.com/astral-sh/uv/issues/17087
synced_at: 2026-01-12T16:02:43Z
```

# `--project <path>` does not fail if `<path>` does not exist

---

_@zanieb_

### Summary

```
❯ uv run --project /tmp/does-not-exist python
Python 3.14.0 (main, Nov 19 2025, 23:04:28) [Clang 21.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

At the very least, I think we should warn?

### Platform

n/a

### Version

n/a

### Python version

n/a

---

_Label `bug` added by @zanieb on 2025-12-11 15:23_

---

_Label `breaking` added by @zanieb on 2025-12-11 15:23_

---
