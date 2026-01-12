```yaml
number: 12471
title: "Fix `Ord` of `cmp_fix`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-ord
created_at: 2024-07-23T07:03:24Z
updated_at: 2024-07-23T13:14:24Z
url: https://github.com/astral-sh/ruff/pull/12471
synced_at: 2026-01-12T15:55:41Z
```

# Fix `Ord` of `cmp_fix`

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/12469


## Test Plan

Running ruff with the given test snipped no longer panics when using rust nightly

> Okay, I think the problem is the first return that higher prioritizes `UnusedImport`. The problem is that it violates
>
> > < is transitive: a < b and b < c implies a < c. The same must hold for both == and >.
>
> Let's say we have
>
> * `a`: `RedefinedWhileUnused` with a min fix of 100
> * `b`: `UnusedImport` with a min fix of 0
> * `c`: any other rule with a min fix of 5
>
> then:
> * `a < b` because of `(Rule::RedefinedWhileUnused, Rule::UnusedImport) => std::cmp::Ordering::Less,`
> * `b < c` because of `.then_with(|| fix1.min_start().cmp(&fix2.min_start()))`
> * but `a > c` `.then_with(|| fix1.min_start().cmp(&fix2.min_start()))`
https://github.com/astral-sh/ruff/issues/12469#issuecomment-2244392085



---

_Review requested from @carljm by @MichaReiser on 2024-07-23 07:03_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-23 07:03_

---

_Label `bug` added by @MichaReiser on 2024-07-23 07:03_

---

_Comment by @github-actions[bot] on 2024-07-23 07:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-07-23 07:50_

---

_Merged by @MichaReiser on 2024-07-23 13:14_

---

_Closed by @MichaReiser on 2024-07-23 13:14_

---

_Branch deleted on 2024-07-23 13:14_

---
