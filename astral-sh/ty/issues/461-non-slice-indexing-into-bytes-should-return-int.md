```yaml
number: 461
title: "Non-slice indexing into `bytes` should return `int`"
type: issue
state: closed
author: grievejia
labels:
  - bug
  - help wanted
  - type-inference
assignees: []
created_at: 2025-05-20T14:04:32Z
updated_at: 2025-05-20T14:44:14Z
url: https://github.com/astral-sh/ty/issues/461
synced_at: 2026-01-12T15:54:23Z
```

# Non-slice indexing into `bytes` should return `int`

---

_@grievejia_

### Summary

Minimal repro:
```python
x = b'abc'
reveal_type(x[0])
```
Expected: `int` or `Literal[97]`
Actual: `Literal[b"a"]`

### Version

ty 0.0.1-alpha.5 (3b726d87a 2025-05-17)

---

_Label `bug` added by @sharkdp on 2025-05-20 14:08_

---

_Label `help wanted` added by @sharkdp on 2025-05-20 14:08_

---

_Label `type-inference` added by @sharkdp on 2025-05-20 14:08_

---

_Comment by @sharkdp on 2025-05-20 14:09_

Thank you for reporting this!

---

_Comment by @InSyncWithFoo on 2025-05-20 14:27_

I'll take this one.

---

_Assigned to @InSyncWithFoo by @MichaReiser on 2025-05-20 14:28_

---

_Closed by @sharkdp on 2025-05-20 14:44_

---
