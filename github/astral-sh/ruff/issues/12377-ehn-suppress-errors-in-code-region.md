---
number: 12377
title: "[EHN] Suppress Errors in Code Region"
type: issue
state: closed
author: randolf-scholz
labels: []
assignees: []
created_at: 2024-07-18T12:06:34Z
updated_at: 2024-07-18T12:09:50Z
url: https://github.com/astral-sh/ruff/issues/12377
synced_at: 2026-01-07T13:12:15-06:00
---

# [EHN] Suppress Errors in Code Region

---

_Issue opened by @randolf-scholz on 2024-07-18 12:06_

Currently, one can suppress errors on a per-line level, per-file level, or globally.

However, sometimes it would be more appropriate to disable errors locally within a file for some region (for instance, suppressing [`ERA001`](https://docs.astral.sh/ruff/rules/commented-out-code/) for some region).

PyLint offers `# pylint: disable=...` and `# pylint: enable=...` for this purpose https://github.com/pylint-dev/pylint/issues/2920#issuecomment-493734595.

Although, this approach is somewhat dangerous imo, since one can accidentally forget to re-enable the error code. So a start/end-region pragma with an automatic check for the existence of the close tag would be ideal.


---

_Comment by @MichaReiser on 2024-07-18 12:09_

That makes sense. We already track this in https://github.com/astral-sh/ruff/issues/3711

---

_Closed by @MichaReiser on 2024-07-18 12:09_

---

_Closed by @MichaReiser on 2024-07-18 12:09_

---
