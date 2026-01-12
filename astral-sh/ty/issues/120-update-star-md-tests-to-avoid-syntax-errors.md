```yaml
number: 120
title: "Update `star.md` tests to avoid syntax errors"
type: issue
state: open
author: ntBre
labels:
  - testing
  - imports
  - internal
assignees: []
created_at: 2025-04-21T13:08:46Z
updated_at: 2025-11-18T16:10:25Z
url: https://github.com/astral-sh/ty/issues/120
synced_at: 2026-01-12T15:54:22Z
```

# Update `star.md` tests to avoid syntax errors

---

_@ntBre_

A few `match` tests in `star.md` have cases like this:

```py
match 42:
    case P | Q:  # error: [invalid-syntax] "name capture `P` makes remaining patterns unreachable"
        ...
```

where the first catch-all makes the second unreachable. @AlexWaygood and I discussed a couple of approaches in [this thread](https://github.com/astral-sh/ruff/pull/17463#discussion_r2050870163), but the best approach that preserved the meaning of the test wasn't immediately clear. We also stumbled upon a new syntax error that I'll add to astral-sh/ruff#17412.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-04-21 13:10_

---

_Renamed from "[red-knot] Update `star.md` tests to avoid syntax errors" to "Update `star.md` tests to avoid syntax errors" by @MichaReiser on 2025-05-07 15:25_

---

_Label `testing` added by @AlexWaygood on 2025-05-10 18:06_

---

_Label `internal` added by @AlexWaygood on 2025-05-10 18:06_

---

_Label `imports` added by @AlexWaygood on 2025-05-10 18:06_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:57_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
