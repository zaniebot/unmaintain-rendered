```yaml
number: 20785
title: "`FURB103` fixes breaks code"
type: issue
state: closed
author: chirizxc
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-09T11:49:04Z
updated_at: 2025-10-31T15:16:11Z
url: https://github.com/astral-sh/ruff/issues/20785
synced_at: 2026-01-12T15:54:57Z
```

# `FURB103` fixes breaks code

---

_@chirizxc_

### Summary

[Playground](https://play.ruff.rs/0d5d7aed-acd4-419e-83f3-623b961c774b) 

This PR breaks: https://github.com/astral-sh/ruff/pull/20520
```python
import json

data = {
    "title": "Fahrenheit 451",
    "price": 100,
}

with open("test.json", "wb") as f:
    f.write(json.dumps(data, indent=4).encode("utf-8"))
```
=>
```python
import json
import pathlib

data = {
    "title": "Fahrenheit 451",
    "price": 100,
}

pathlib.Path("test.json").write_bytes(json.dumps(indent=4, data).encode("utf-8"))

# File "test.py", line 9
# pathlib.Path("test.json").write_bytes(json.dumps(indent=4, data).encode("utf-8"))
#                                                                  ^
# SyntaxError: positional argument follows keyword argument
```

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Comment by @chirizxc on 2025-10-09 11:50_

@ntBre can you assign me?

---

_Assigned to @chirizxc by @ntBre on 2025-10-09 13:28_

---

_Label `bug` added by @ntBre on 2025-10-09 13:28_

---

_Label `fixes` added by @ntBre on 2025-10-09 13:28_

---

_Closed by @ntBre on 2025-10-31 15:16_

---
