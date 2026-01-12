```yaml
number: 1611
title: "panic: `assertion failed: start.raw <= end.raw` with unclosed forward reference"
type: issue
state: closed
author: correctmost
labels:
  - fatal
assignees: []
created_at: 2025-11-23T11:32:50Z
updated_at: 2025-11-23T15:51:59Z
url: https://github.com/astral-sh/ty/issues/1611
synced_at: 2026-01-12T15:54:25Z
```

# panic: `assertion failed: start.raw <= end.raw` with unclosed forward reference

---

_@correctmost_

### Summary

ty panics when checking this code:

```python
a:'
```

```
error[panic]: Panicked at crates/ruff_python_ast/src/nodes.rs:1657:9 when checking `/tmp/foo.py`: `assertion failed: start.raw <= end.raw`
```



### Version

ty 0.0.1-alpha.27

---

_Label `fatal` added by @MichaReiser on 2025-11-23 11:49_

---

_Closed by @MichaReiser on 2025-11-23 15:51_

---
