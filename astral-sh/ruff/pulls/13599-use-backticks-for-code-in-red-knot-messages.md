```yaml
number: 13599
title: Use backticks for code in red-knot messages
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ticks
created_at: 2024-10-02T02:15:08Z
updated_at: 2024-10-02T03:28:26Z
url: https://github.com/astral-sh/ruff/pull/13599
synced_at: 2026-01-12T15:55:44Z
```

# Use backticks for code in red-knot messages

---

_@charliermarsh_

## Summary

...and remove periods from messages that don't span more than a single sentence.

This is more consistent with how we present user-facing messages in uv (which has a defined style guide).


---

_Review requested from @carljm by @charliermarsh on 2024-10-02 02:15_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-02 02:15_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-02 02:15_

---

_Comment by @charliermarsh on 2024-10-02 02:17_

I might also suggest removing periods from the end of these error messages (unless they span more than one sentence). We use that policy in uv and in Ruff.

---

_@carljm approved on 2024-10-02 02:24_

Thanks!

I have no problem with removing final periods, in this PR or another one; it makes sense to have a consistent style guide across projects. 

---

_Comment by @carljm on 2024-10-02 02:25_

Looks like there's one missed test to fix. 

---

_Comment by @charliermarsh on 2024-10-02 02:28_

Sounds good. I'll probably just bundle it here then to reduce churn (fewer commits to ignore when git blaming).

---

_Comment by @github-actions[bot] on 2024-10-02 03:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2024-10-02 03:14_

---

_Closed by @charliermarsh on 2024-10-02 03:14_

---

_Branch deleted on 2024-10-02 03:14_

---
