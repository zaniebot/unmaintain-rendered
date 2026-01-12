```yaml
number: 19861
title: "Add pycharm like check rule about \"Remove redundant parentheses\""
type: issue
state: closed
author: andyxning
labels: []
assignees: []
created_at: 2025-08-11T05:22:34Z
updated_at: 2025-08-11T07:35:37Z
url: https://github.com/astral-sh/ruff/issues/19861
synced_at: 2026-01-12T15:54:57Z
```

# Add pycharm like check rule about "Remove redundant parentheses"

---

_@andyxning_

### Summary

Pycharm by default will warning about `Remove redundant parentheses` just like this:

```python

def a() -> tuple[int, int]:
    return (1, 1)
```

---

_Comment by @MichaReiser on 2025-08-11 07:35_

I'll merge this into an existing issue that also requests for a rule that flags redundant parentheses (they use a different example but the request is the same)

---

_Closed by @MichaReiser on 2025-08-11 07:35_

---
