---
number: 15866
title: "[red-knot] Revisit diagnostic range for unresolved-import"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
  - diagnostics
assignees: []
created_at: 2025-02-01T08:53:53Z
updated_at: 2025-02-05T19:01:59Z
url: https://github.com/astral-sh/ruff/issues/15866
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Revisit diagnostic range for unresolved-import

---

_Issue opened by @MichaReiser on 2025-02-01 08:53_

### Description

See my comment here https://github.com/astral-sh/ruff/pull/15856/files#r1938233453

We should probably only highlight the module name when resolving a `from ...` import` fails

---

_Label `diagnostics` added by @MichaReiser on 2025-02-01 08:53_

---

_Label `red-knot` added by @MichaReiser on 2025-02-01 08:53_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-02-03 15:51_

---

_Comment by @BurntSushi on 2025-02-03 15:52_

I'd like to take a crack at this one. I think it would be a good exercise for me to go through the process of an actual improvement here by dipping my toes into it. I'll time box this to an hour or two, so if it proves too challenging, I'll back off of it.

---

_Referenced in [astral-sh/ruff#15976](../../astral-sh/ruff/pulls/15976.md) on 2025-02-05 18:10_

---

_Closed by @BurntSushi on 2025-02-05 19:01_

---

_Closed by @BurntSushi on 2025-02-05 19:01_

---
