---
number: 19663
title: "`D419`: Add unsafe fix for removing empty docstrings"
type: issue
state: open
author: cometbeetle
labels:
  - fixes
  - needs-decision
assignees: []
created_at: 2025-07-31T13:42:28Z
updated_at: 2025-07-31T14:33:08Z
url: https://github.com/astral-sh/ruff/issues/19663
synced_at: 2026-01-07T13:12:16-06:00
---

# `D419`: Add unsafe fix for removing empty docstrings

---

_Issue opened by @cometbeetle on 2025-07-31 13:42_

### Summary

Hello!

I noticed with `D419` that there is no unsafe fix for simply removing empty docstrings that are detected. I think this would be a simple yet useful feature, and was wondering about the feasibility of adding it. 

---

_Renamed from "`D419`: Add unsafe fix for empty docstrings" to "`D419`: Add unsafe fix for removing empty docstrings" by @cometbeetle on 2025-07-31 13:42_

---

_Label `fixes` added by @ntBre on 2025-07-31 14:27_

---

_Label `needs-decision` added by @ntBre on 2025-07-31 14:27_

---

_Comment by @ntBre on 2025-07-31 14:33_

Hmm, I'm not sure how helpful such a fix would be. I feel like an empty docstring is more of an indicator that someone forgot to come back and fill it in rather than forgot to delete it. But I'm certainly open to other opinions!

---
