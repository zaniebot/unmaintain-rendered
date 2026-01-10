```yaml
number: 19690
title: "Use new `Diagnostic` functionality for Ruff rules"
type: issue
state: closed
author: ntBre
labels:
  - tracking
  - diagnostics
assignees: []
created_at: 2025-08-01T15:16:13Z
updated_at: 2025-08-14T19:43:28Z
url: https://github.com/astral-sh/ruff/issues/19690
synced_at: 2026-01-10T11:09:59Z
```

# Use new `Diagnostic` functionality for Ruff rules

---

_Issue opened by @ntBre on 2025-08-01 15:16_

It's a bit late in the process, but it seemed useful to write up a tracking issue for the remaining TODOs before we can take advantage of our new `Diagnostic` infrastructure in Ruff. This is primarily focused on the `full` output format, which is where the new functionality like multiple annotations will be visible. We've already switched over the `concise` and a few other formats without any user-visible changes.

In brief, the remaining tasks are to switch Ruff over to the new `full`-format rendering code in `ruff_db`, update our caching code to preserve all of the information in the new diagnostics instead of the legacy subset (or sidestep this by avoiding the cache entirely in those cases, as we ended up doing), and then actually use some of the new functionality in real lint rules. The first of these has been broken into a few subtasks to make it easier for review.

- [x] https://github.com/astral-sh/ruff/pull/19415
  - [x] https://github.com/astral-sh/ruff/pull/19644
  - [x] https://github.com/astral-sh/ruff/pull/19653
  - [x] https://github.com/astral-sh/ruff/pull/19698
- [x] https://github.com/astral-sh/ruff/pull/19869
- [ ] https://github.com/astral-sh/ruff/issues/19688
- [ ] https://github.com/astral-sh/ruff/issues/17203

---

_Assigned to @ntBre by @ntBre on 2025-08-01 15:16_

---

_Label `tracking` added by @ntBre on 2025-08-01 15:16_

---

_Label `diagnostics` added by @ntBre on 2025-08-01 15:16_

---

_Comment by @ntBre on 2025-08-14 19:43_

I mostly wanted to track the stack of rendering PRs and then caching. The remaining tasks seem well-covered by their own issues.

---

_Closed by @ntBre on 2025-08-14 19:43_

---
