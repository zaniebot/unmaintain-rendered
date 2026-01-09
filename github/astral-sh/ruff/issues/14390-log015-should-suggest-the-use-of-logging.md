---
number: 14390
title: "[LOG015] should suggest the use of `logging.getLogger(__name__)`"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - rule
  - preview
assignees: []
created_at: 2024-11-16T21:54:46Z
updated_at: 2024-11-17T08:22:53Z
url: https://github.com/astral-sh/ruff/issues/14390
synced_at: 2026-01-07T13:12:16-06:00
---

# [LOG015] should suggest the use of `logging.getLogger(__name__)`

---

_Issue opened by @MichaReiser on 2024-11-16 21:54_

> The recently implemented [LOG015](https://docs.astral.sh/ruff/rules/root-logger-call/#example) in #14302 is in conflict with [LOG002](https://docs.astral.sh/ruff/rules/invalid-get-logger-argument/#example). One suggests using `logging.getLogger(__file__)`, the other calls that a bad practice. LOG015 should suggest `logging.getLogger(__name__)`.

It also seems to be triggering when `logging.getLogger(__name__)` is already being used, ie, not a root logger.

_Originally posted by @qartik in https://github.com/astral-sh/ruff/issues/7248#issuecomment-2480563435_

LOG015 incorrectly suggests the usage of `logging.getLogger(__file__)` over ``logging.getLogger(__name__)`. 


            

---

_Label `bug` added by @MichaReiser on 2024-11-16 21:55_

---

_Label `rule` added by @MichaReiser on 2024-11-16 21:55_

---

_Label `preview` added by @MichaReiser on 2024-11-16 21:55_

---

_Comment by @MichaReiser on 2024-11-16 21:55_

@InSyncWithFoo are you interested in taking a look at the fix?

---

_Referenced in [astral-sh/ruff#14392](../../astral-sh/ruff/pulls/14392.md) on 2024-11-17 00:30_

---

_Comment by @InSyncWithFoo on 2024-11-17 00:32_

Submitted a PR. An easy fix, as only the doc comment is affected.

---

_Closed by @MichaReiser on 2024-11-17 08:22_

---
