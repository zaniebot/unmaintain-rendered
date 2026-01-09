---
number: 15292
title: "Should `eq-without-hash (PLW1641)` not apply to stubs?"
type: issue
state: closed
author: Avasam
labels:
  - rule
  - preview
assignees: []
created_at: 2025-01-06T03:29:39Z
updated_at: 2025-01-07T02:51:08Z
url: https://github.com/astral-sh/ruff/issues/15292
synced_at: 2026-01-07T13:12:16-06:00
---

# Should `eq-without-hash (PLW1641)` not apply to stubs?

---

_Issue opened by @Avasam on 2025-01-06 03:29_

I don't think that [eq-without-hash (PLW1641)](https://docs.astral.sh/ruff/rules/eq-without-hash/#eq-without-hash-plw1641) should apply to stubs (it currently does).
Stubs authors should aim to faithfully represent the runtime implementation. And if the runtime doesn't implement `__hash__`, the stubs shouldn't define it either.

Third-party stub authors have no control on this. First-party stubs would get `FURB189` in their source `.py` files.

Ruff: 0.8.5 (rule is currently in preview)

(this report is extracted from https://github.com/astral-sh/ruff/issues/14535#issuecomment-2561461038 for ease of tracking and discussion)

---

_Referenced in [astral-sh/ruff#14535](../../astral-sh/ruff/issues/14535.md) on 2025-01-06 03:30_

---

_Label `question` added by @dhruvmanila on 2025-01-06 06:05_

---

_Label `rule` added by @MichaReiser on 2025-01-06 07:47_

---

_Label `preview` added by @MichaReiser on 2025-01-06 07:48_

---

_Label `question` removed by @MichaReiser on 2025-01-06 07:48_

---

_Comment by @MichaReiser on 2025-01-06 07:48_

I think that would make sense to me. 


---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-07 02:43_

---

_Referenced in [astral-sh/ruff#15310](../../astral-sh/ruff/pulls/15310.md) on 2025-01-07 02:45_

---

_Closed by @charliermarsh on 2025-01-07 02:51_

---
