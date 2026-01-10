```yaml
number: 8913
title: "Incorrect range for `WithItem` node"
type: issue
state: closed
author: LaBatata101
labels:
  - bug
  - parser
assignees: []
created_at: 2023-11-29T23:31:12Z
updated_at: 2023-11-30T00:11:05Z
url: https://github.com/astral-sh/ruff/issues/8913
synced_at: 2026-01-10T11:09:51Z
```

# Incorrect range for `WithItem` node

---

_Issue opened by @LaBatata101 on 2023-11-29 23:31_

Ruff version: v0.1.6

In the following code snippet
```python
with (a := b) as x: pass
```

The `WithItem` range is `6..17` which contains this part of the source code `a := b) as`, as you can see the left parenthesis and the `x` is missing from the range. The correct range would be `5..18`.

[Here is the AST](https://play.ruff.rs/f34cef2d-7ccc-410d-b90d-60ff655f6c60).

---

_Label `bug` added by @MichaReiser on 2023-11-29 23:37_

---

_Label `parser` added by @MichaReiser on 2023-11-29 23:37_

---

_Comment by @charliermarsh on 2023-11-29 23:49_

Odd, I can take a look at this.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-30 00:03_

---

_Closed by @charliermarsh on 2023-11-30 00:11_

---
