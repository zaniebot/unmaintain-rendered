---
number: 21313
title: Rules D201 and D202 have a confusing name
type: issue
state: open
author: Wim-De-Clercq
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-11-07T09:10:30Z
updated_at: 2025-11-07T19:02:41Z
url: https://github.com/astral-sh/ruff/issues/21313
synced_at: 2026-01-07T13:12:16-06:00
---

# Rules D201 and D202 have a confusing name

---

_Issue opened by @Wim-De-Clercq on 2025-11-07 09:10_

### Summary

[D201](https://docs.astral.sh/ruff/rules/blank-line-before-function/) and [D202](https://docs.astral.sh/ruff/rules/blank-line-after-function/) are called `blank-line-before-function` and `blank-line-after-function`

That makes the rules sound like they look for blank lines around the function definition. However, they are rules for the docstrings.

I would suggest adding the suffix `-docstring` to the names

---

_Renamed from "D201 and D202 have a confusing name" to "Rules D201 and D202 have a confusing name" by @Wim-De-Clercq on 2025-11-07 09:15_

---

_Comment by @ntBre on 2025-11-07 19:02_

Hmm, I can see what you mean. I guess the pydocstyle category helps right now, but we could consider making this change when working on #1774 especially.

---

_Label `rule` added by @ntBre on 2025-11-07 19:02_

---

_Label `needs-decision` added by @ntBre on 2025-11-07 19:02_

---
