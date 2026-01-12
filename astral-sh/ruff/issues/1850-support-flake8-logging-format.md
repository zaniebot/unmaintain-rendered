```yaml
number: 1850
title: Support flake8-logging-format ?
type: issue
state: closed
author: neutrinoceros
labels:
  - plugin
assignees: []
created_at: 2023-01-13T14:16:12Z
updated_at: 2023-01-26T17:59:28Z
url: https://github.com/astral-sh/ruff/issues/1850
synced_at: 2026-01-12T15:54:41Z
```

# Support flake8-logging-format ?

---

_@neutrinoceros_

Hi, thanks a lot for making ruff, I'm amazed by the quality and performances of this fantastic tool !

[`flake8-logging-format`](https://pypi.org/project/flake8-logging-format) isn't crucial to my workflow, but it's the only flake8 plugin I use that isn't supported yet *and* doesn't have an open ticket to follow already so I figured I'd open one for reference.

Keep it up !

---

_Label `plugin` added by @charliermarsh on 2023-01-13 16:47_

---

_Comment by @edgarrmondragon on 2023-01-25 08:32_

I'm working on some of these. For tracking:

- [x] `G001`: Logging statements should not use `string.format()` for their first argument
- [x] `G002`: Logging statements should not use `%` formatting for their first argument
- [x] `G003`: Logging statements should not use `+` concatenation for their first argument
- [x] `G004`: Logging statements should not use `f"..."` for their first argument (only in Python 3.6+)
- [x] `G010`: Logging statements should not use `warn` (use warning instead)
- [ ] `G100`: Logging statements should not use `extra` arguments unless whitelisted
- [x] `G101`: Logging statement should not use `extra` arguments that clash with LogRecord fields
- [ ] `G200`: Logging statements should not include the exception in logged string (use `exception` or `exc_info=True`)
- [x] `G201`: Logging statements should not use `error(..., exc_info=True)` (use `exception(...)` instead)
- [x] `G202`: Logging statements should not use redundant `exc_info=True` in `exception`


---

_Comment by @charliermarsh on 2023-01-26 17:59_

I'm going to tentatively close this, and we can open issues for those two remaining rules as they come up.

---

_Closed by @charliermarsh on 2023-01-26 17:59_

---
