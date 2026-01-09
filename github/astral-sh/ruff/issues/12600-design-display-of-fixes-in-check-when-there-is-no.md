---
number: 12600
title: "Design display of fixes in `check` when there is no source"
type: issue
state: open
author: chriskrycho
labels:
  - fixes
  - cli
  - diagnostics
assignees: []
created_at: 2024-07-31T18:15:59Z
updated_at: 2025-01-21T13:45:12Z
url: https://github.com/astral-sh/ruff/issues/12600
synced_at: 2026-01-07T13:12:15-06:00
---

# Design display of fixes in `check` when there is no source

---

_Issue opened by @chriskrycho on 2024-07-31 18:15_

Spun off from #12595: in some cases, there may be a possible fix but no source, e.g. when passing no input at all (see [here](https://github.com/astral-sh/ruff/pull/12595/files/b824fd328a997e06038aaa8627e06303031f270e#diff-17ba716421a75d2bd60e7328e4b0f368736d61bab68142ac7642d1bfe57efe32) from an earlier version of the PR than the one that landed). Most likely, those fixes should not be emitted at all. However, this needs a bit of design: maybe it *is* legitimate in some cases; and/or perhaps the decision should happen upstream to avoid creating the fix at all?

---

_Referenced in [astral-sh/ruff#12595](../../astral-sh/ruff/pulls/12595.md) on 2024-07-31 18:16_

---

_Label `fixes` added by @AlexWaygood on 2024-07-31 18:16_

---

_Label `cli` added by @dhruvmanila on 2024-08-01 06:08_

---

_Label `diagnostics` added by @MichaReiser on 2025-01-21 13:45_

---

_Referenced in [astral-sh/ruff#19919](../../astral-sh/ruff/pulls/19919.md) on 2025-08-14 19:41_

---
