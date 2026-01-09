---
number: 7710
title: Consider removing PD901
type: issue
state: closed
author: charliermarsh
labels:
  - breaking
assignees: []
created_at: 2023-09-29T12:04:52Z
updated_at: 2025-09-05T14:24:19Z
url: https://github.com/astral-sh/ruff/issues/7710
synced_at: 2026-01-07T13:12:15-06:00
---

# Consider removing PD901

---

_Issue opened by @charliermarsh on 2023-09-29 12:04_

PD901 disallows `df` as a variable name. This is an opinionated rule in `pandas-vet` so is typically only enabled via explicit opt-in. But we don't have that behavior for 900-level rules, so it's always enabled if you enable `PD`. The rule is overly strict for most cases, so I think it's a net-negative to have it in Ruff given our behavior.

---

_Label `needs-decision` added by @charliermarsh on 2023-09-29 12:04_

---

_Comment by @dhruvmanila on 2023-10-04 04:19_

I'm in favor of this 👍

---

_Referenced in [astral-sh/ruff#9461](../../astral-sh/ruff/issues/9461.md) on 2024-01-11 01:22_

---

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:21_

---

_Comment by @zanieb on 2024-01-30 01:24_

@charliermarsh can you fill this issue in?

---

_Referenced in [astral-sh/ruff#9680](../../astral-sh/ruff/pulls/9680.md) on 2024-01-30 19:18_

---

_Closed by @zanieb on 2024-02-01 19:35_

---

_Comment by @kdebrab on 2024-09-27 05:54_

AFAICT, it seems like rule PD901 has not been deprecated?

---

_Comment by @dhruvmanila on 2024-10-01 04:35_

Yeah, it wasn't removed. cc @charliermarsh

---

_Comment by @charliermarsh on 2024-10-01 04:38_

I don't think I closed it! But I'll re-open.

---

_Reopened by @charliermarsh on 2024-10-01 04:38_

---

_Comment by @zanieb on 2024-10-01 05:02_

Can you fill in the context on why we should consider removing the rule?

---

_Comment by @charliermarsh on 2024-10-01 16:04_

Sure.

---

_Label `needs-decision` removed by @charliermarsh on 2024-10-01 16:04_

---

_Label `breaking` added by @charliermarsh on 2024-10-01 16:04_

---

_Comment by @icp1994 on 2025-03-21 08:14_

Hi, can this be considered for v0.12?

---

_Removed from milestone `v0.2.0` by @MichaReiser on 2025-03-21 08:34_

---

_Added to milestone `v0.11` by @MichaReiser on 2025-03-21 08:34_

---

_Referenced in [astral-sh/ruff#18618](../../astral-sh/ruff/pulls/18618.md) on 2025-06-10 20:36_

---

_Comment by @MichaReiser on 2025-06-13 06:45_

We'll deprecate this rule in 0.12 and it's scheduled for removal in 0.13

---

_Removed from milestone `v0.12` by @MichaReiser on 2025-06-13 06:45_

---

_Added to milestone `v0.13` by @MichaReiser on 2025-06-13 06:45_

---

_Referenced in [astral-sh/ruff#19223](../../astral-sh/ruff/pulls/19223.md) on 2025-07-09 01:32_

---

_Closed by @ntBre on 2025-09-05 14:24_

---
