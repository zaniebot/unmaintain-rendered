---
number: 7349
title: "CLI: Show source context by default in `ruff check`"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2023-09-13T16:08:27Z
updated_at: 2024-06-26T09:02:19Z
url: https://github.com/astral-sh/ruff/issues/7349
synced_at: 2026-01-07T13:12:15-06:00
---

# CLI: Show source context by default in `ruff check`

---

_Issue opened by @zanieb on 2023-09-13 16:08_

The `--show-source` flag can be used to show source information alongside violations during `ruff check`. This should be the default behavior.

`--no-show-source` can be used to return to the previous output. In the future, this option will be removed; see #7350.

---

_Label `cli` added by @zanieb on 2023-09-13 16:08_

---

_Comment by @MichaReiser on 2023-09-13 16:10_

How does this align with our long-term goal of removing `--show-source` in favor of different formats? 

---

_Referenced in [astral-sh/ruff#7350](../../astral-sh/ruff/issues/7350.md) on 2023-09-13 16:11_

---

_Referenced in [astral-sh/ruff#7352](../../astral-sh/ruff/issues/7352.md) on 2023-09-13 16:21_

---

_Referenced in [astral-sh/ruff#7353](../../astral-sh/ruff/issues/7353.md) on 2023-09-13 16:25_

---

_Referenced in [astral-sh/ruff#9687](../../astral-sh/ruff/pulls/9687.md) on 2024-01-29 19:21_

---

_Added to milestone `v0.3.0` by @zanieb on 2024-01-30 00:17_

---

_Removed from milestone `v0.3.0` by @MichaReiser on 2024-02-14 16:27_

---

_Added to milestone `v0.4` by @MichaReiser on 2024-02-14 16:27_

---

_Comment by @charliermarsh on 2024-02-25 21:30_

This is already the case in preview, right?

---

_Comment by @zanieb on 2024-02-25 23:07_

Yeah I believe this was completed in #9687

Should we leave the issue open to track making it the non-preview default?

---

_Referenced in [astral-sh/ruff#9814](../../astral-sh/ruff/pulls/9814.md) on 2024-04-15 13:48_

---

_Removed from milestone `v0.4.0` by @dhruvmanila on 2024-04-18 19:48_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-04-18 19:48_

---

_Referenced in [astral-sh/ruff#12010](../../astral-sh/ruff/pulls/12010.md) on 2024-06-24 09:06_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-25 09:18_

---

_Removed from milestone `v0.5.0` by @MichaReiser on 2024-06-26 08:12_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-06-26 08:12_

---

_Removed from milestone `v0.6` by @MichaReiser on 2024-06-26 08:12_

---

_Added to milestone `v0.5.0` by @MichaReiser on 2024-06-26 08:12_

---

_Comment by @T-256 on 2024-06-26 08:36_

IIUC `--output-fomat=full` as default is stabilized in v0.5, why keeping this issue for v0.6 milestone?

---

_Referenced in [astral-sh/ruff#12005](../../astral-sh/ruff/pulls/12005.md) on 2024-06-26 08:46_

---

_Closed by @MichaReiser on 2024-06-26 09:02_

---
