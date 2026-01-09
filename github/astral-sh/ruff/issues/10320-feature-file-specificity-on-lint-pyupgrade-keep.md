---
number: 10320
title: "Feature: file-specificity on `lint.pyupgrade.keep-runtime-typing`"
type: issue
state: closed
author: tlambert03
labels:
  - configuration
assignees: []
created_at: 2024-03-10T16:23:23Z
updated_at: 2024-03-10T23:37:23Z
url: https://github.com/astral-sh/ruff/issues/10320
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature: file-specificity on `lint.pyupgrade.keep-runtime-typing`

---

_Issue opened by @tlambert03 on 2024-03-10 16:23_

Not sure if I missed an already-possible feature, but I often find myself working in libraries that need `keep-runtime-typing = True` for only a select few files.  It would be convenient, in those cases, to enforce the rule for a subset of files with something like:

```toml
[tool.ruff.lint.pyupgrade]
keep-runtime-typing-files = ["src/subfolder/*.py"]
```

this is not currently possible, right?

---

_Comment by @charliermarsh on 2024-03-10 23:37_

It's not currently possible but we do want to support it, probably in a more general way like https://github.com/astral-sh/ruff/issues/7696. I'm gonna merge into that issue!

---

_Closed by @charliermarsh on 2024-03-10 23:37_

---

_Label `configuration` added by @charliermarsh on 2024-03-10 23:37_

---
