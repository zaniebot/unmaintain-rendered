---
number: 1062
title: "[bug?] `F401` (unused import) false negative in try block"
type: issue
state: closed
author: smackesey
labels: []
assignees: []
created_at: 2022-12-05T14:54:45Z
updated_at: 2022-12-05T14:55:44Z
url: https://github.com/astral-sh/ruff/issues/1062
synced_at: 2026-01-10T01:22:38Z
---

# [bug?] `F401` (unused import) false negative in try block

---

_Issue opened by @smackesey on 2022-12-05 14:54_

Even though `JSONDecodeError` isn't used anywhere else, this does not trigger `F401`:

```
try:
    from json import JSONDecodeError 
except ImportError:
    JSONDecodeError = ValueError  # type: ignore[misc, assignment]
```

I confirmed `F401` is triggered when moved out of the block. 

`flake8` has the same behavior-- not sure if it is a bug or intended, but if intended it should be documented (maybe it is? But I couldn't find it).

Update: never mind, I see why it doesn't throw.

---

_Closed by @smackesey on 2022-12-05 14:55_

---
