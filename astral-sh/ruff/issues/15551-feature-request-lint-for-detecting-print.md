```yaml
number: 15551
title: Feature request - lint for detecting print statements
type: issue
state: closed
author: brass75
labels:
  - question
assignees: []
created_at: 2025-01-17T14:06:50Z
updated_at: 2025-01-17T14:30:48Z
url: https://github.com/astral-sh/ruff/issues/15551
synced_at: 2026-01-10T11:09:57Z
```

# Feature request - lint for detecting print statements

---

_Issue opened by @brass75 on 2025-01-17 14:06_

The projects I tend to write (as many) do not use `print` in production code however we do sometimes use them in debugging. It would be great if there was a lint we could turn on to detect forgotten `print` statements.

---

_Comment by @MichaReiser on 2025-01-17 14:15_

Is [print-found](https://docs.astral.sh/ruff/rules/print/) what you're looking for?

---

_Label `question` added by @MichaReiser on 2025-01-17 14:15_

---

_Comment by @brass75 on 2025-01-17 14:30_

It is! Thanks!

---

_Closed by @brass75 on 2025-01-17 14:30_

---
