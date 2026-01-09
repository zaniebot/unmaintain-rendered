---
number: 11480
title: docstring-code-format incorrectly formats semicolons+expressions
type: issue
state: open
author: JasonGrace2282
labels:
  - bug
  - docstring
  - formatter
assignees: []
created_at: 2024-05-21T00:14:45Z
updated_at: 2024-05-21T21:01:15Z
url: https://github.com/astral-sh/ruff/issues/11480
synced_at: 2026-01-07T13:12:15-06:00
---

# docstring-code-format incorrectly formats semicolons+expressions

---

_Issue opened by @JasonGrace2282 on 2024-05-21 00:14_

## Example
```py
"""
>>> l = []
>>> l.append(3); l
[3]
"""
```
### Expected
```py
"""
>>> l = []
>>> l.append(3)
>>> l
[3]
"""
```

### Result
```py
"""
>>> l = []
>>> l.append(3)
... l
[3]
"""
```

## Reproduce:
```toml
[tool.ruff.format]
docstring-code-format = true
```
And run `ruff format` on the file.

### Ruff Version
```bash
$ ruff -V
ruff 0.4.4
```

---

_Renamed from "docstring-code-format fails with semicolons" to "docstring-code-format fails with semicolons+expressions" by @JasonGrace2282 on 2024-05-21 00:15_

---

_Label `bug` added by @charliermarsh on 2024-05-21 19:07_

---

_Label `docstring` added by @charliermarsh on 2024-05-21 19:07_

---

_Label `formatter` added by @charliermarsh on 2024-05-21 19:07_

---

_Comment by @charliermarsh on 2024-05-21 19:08_

Interesting. Is that actually an _error_, or just suboptimal formatting? I can't remember what the execution semantics are of doctests like that.

---

_Comment by @JasonGrace2282 on 2024-05-21 20:59_

Ah, bad phrasing on my part. It's simply incorrect formatting.

Edit: I misread your question. Running the doctest fails.

---

_Renamed from "docstring-code-format fails with semicolons+expressions" to "docstring-code-format incorrectly formats semicolons+expressions" by @JasonGrace2282 on 2024-05-21 21:00_

---

_Comment by @charliermarsh on 2024-05-21 21:00_

Thanks for the follow-up. The request makes sense to me.

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-09-16 16:19_

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:37_

---
