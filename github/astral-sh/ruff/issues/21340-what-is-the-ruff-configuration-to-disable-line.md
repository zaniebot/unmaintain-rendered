---
number: 21340
title: What is the Ruff configuration to disable line wrapping?
type: issue
state: closed
author: cos4ni2s
labels:
  - question
assignees: []
created_at: 2025-11-08T15:13:47Z
updated_at: 2025-11-08T15:56:20Z
url: https://github.com/astral-sh/ruff/issues/21340
synced_at: 2026-01-07T13:12:16-06:00
---

# What is the Ruff configuration to disable line wrapping?

---

_Issue opened by @cos4ni2s on 2025-11-08 15:13_

### Question

I need to disable this automatic line-wrapping behavior entirely or, at least, set the line length limit higher than the default.

I've checked the documentation but haven't found the specific configuration key that controls this aggressive splitting.

### Version

_No response_

---

_Label `question` added by @cos4ni2s on 2025-11-08 15:13_

---

_Comment by @MichaReiser on 2025-11-08 15:25_

See https://docs.astral.sh/ruff/settings/#line-length or only run `ruff check` and not `ruff format`

---

_Comment by @cos4ni2s on 2025-11-08 15:41_

Thank you.
I was looking at settings under `[format]`

---

_Closed by @MichaReiser on 2025-11-08 15:56_

---
