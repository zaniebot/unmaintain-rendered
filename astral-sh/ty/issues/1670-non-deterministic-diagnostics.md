---
number: 1670
title: Non deterministic diagnostics
type: issue
state: open
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2025-11-28T21:28:52Z
updated_at: 2026-01-09T15:37:44Z
url: https://github.com/astral-sh/ty/issues/1670
synced_at: 2026-01-10T01:48:23Z
---

# Non deterministic diagnostics

---

_Issue opened by @MichaReiser on 2025-11-28 21:28_

### Summary

Ecosystem reports now often show flaky diffs, for example https://github.com/astral-sh/ruff/pull/21595#issuecomment-3568122741. Sometimes it's flaky diagnostics, sometimes it's only the ordering of union elements that changes between runs.


This has become more prevalent since https://github.com/astral-sh/ruff/pull/20566 landed. 

We need deterministic diagnostic output (including messages) for GitLab output reports (or baselines)

A PR fixing the instability should also enable the CI job added in https://github.com/astral-sh/ruff/pull/21864, demonstrating that the instability is fixed

### Version

_No response_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-28 21:28_

---

_Label `bug` added by @MichaReiser on 2025-11-28 21:28_

---

_Comment by @mtshiba on 2025-11-29 03:21_

The simplest solution is to sort the union types in the cycle recovery function. We can also do this for recursively defined types only, as in https://github.com/astral-sh/ruff/pull/21683.

But honestly, I don't really understand why this happens. Is it a flaw in our implementation, as in https://github.com/astral-sh/ruff/pull/20966, or is it a salsa specification or bug? It seems I need to look into this in more detail.

---

_Comment by @MichaReiser on 2025-11-29 14:53_

Some notes from our internal Discord channel on why this might be happening https://discord.com/channels/1039017663004942429/1443381998100938783/1443492249307582484

I don't think this is a Salsa bug. It's just that some queries are execution order dependent and there's no deterministic execution order for cycles (because they can be entered from many different queries, running on multiple threads at the same time)

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:38_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:38_

---

_Assigned to @carljm by @carljm on 2026-01-09 15:32_

---

_Assigned to @mtshiba by @carljm on 2026-01-09 15:32_

---

_Assigned to @oconnor663 by @carljm on 2026-01-09 15:37_

---

_Unassigned @carljm by @carljm on 2026-01-09 15:37_

---
