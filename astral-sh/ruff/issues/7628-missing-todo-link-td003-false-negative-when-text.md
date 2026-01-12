```yaml
number: 7628
title: "`missing-todo-link` (`TD003`) - false negative when text preceeding \"TODO\""
type: issue
state: open
author: DetachHead
labels:
  - needs-decision
assignees: []
created_at: 2023-09-24T11:39:47Z
updated_at: 2023-09-24T14:18:00Z
url: https://github.com/astral-sh/ruff/issues/7628
synced_at: 2026-01-12T15:54:47Z
```

# `missing-todo-link` (`TD003`) - false negative when text preceeding "TODO"

---

_@DetachHead_

```py
# asdf TODO: no error here
# TODO: error
```

---

_Comment by @charliermarsh on 2023-09-24 14:17_

This may need to be declared a reasonable limitation, because there's always going to be a tradeoff here between false negatives and false positives. If we detect `TD` rules here, it'll be impossible to use `TODO` in prose, like:

```python
# This solves case X.
# We don't solve case Y. That's left as a TODO for future work.
```

The limitation we impose (IIUC) is that the TODO has to be at the start of the comment. I'm not sure we can rely on the colon either (i.e., detect any TODO followed by a colon, even if it's not at the start) because we have rules that detect the lack of colon, so it would be inconsistent in enforcement for those cases.

Any ideas?

---

_Label `needs-decision` added by @charliermarsh on 2023-09-24 14:18_

---
