```yaml
number: 14033
title: Use named function in incremental red knot benchmark
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/red-knot-incremental
created_at: 2024-11-01T08:39:01Z
updated_at: 2024-11-01T08:53:42Z
url: https://github.com/astral-sh/ruff/pull/14033
synced_at: 2026-01-10T20:59:37Z
```

# Use named function in incremental red knot benchmark

---

_Pull request opened by @MichaReiser on 2024-11-01 08:39_

## Summary

I multiple times made the mistake to look at the "warmup" time when analysing the incremental benchmark time instead of the
incremental time because I only looked at where most time was spent. 

This PR uses named functions for the setup and incremental phases instead of unnamed closures to make it
evident in profiles whether one is looking at one or the other.




---

_Label `red-knot` added by @MichaReiser on 2024-11-01 08:39_

---

_Merged by @MichaReiser on 2024-11-01 08:44_

---

_Closed by @MichaReiser on 2024-11-01 08:44_

---

_Branch deleted on 2024-11-01 08:44_

---

_Comment by @github-actions[bot] on 2024-11-01 08:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
