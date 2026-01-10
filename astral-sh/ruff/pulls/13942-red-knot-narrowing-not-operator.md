```yaml
number: 13942
title: "[red-knot] Narrowing - Not operator "
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/narrow-not-expression
created_at: 2024-10-27T20:32:08Z
updated_at: 2024-10-28T20:49:18Z
url: https://github.com/astral-sh/ruff/pull/13942
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Narrowing - Not operator 

---

_Pull request opened by @TomerBin on 2024-10-27 20:32_

## Summary

After #13918 has landed, narrowing constraint negation became easy, so adding support for `not` operator.

## Test Plan

I added a new mdtest file for `not` expression, but maybe we should spread it over the other test files (by adding `not` cases to each one of them)? I'll be happy to hear your preference around that :)




---

_Comment by @github-actions[bot] on 2024-10-27 20:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @TomerBin on 2024-10-27 21:30_

---

_Review requested from @carljm by @TomerBin on 2024-10-27 21:30_

---

_Review requested from @MichaReiser by @TomerBin on 2024-10-27 21:30_

---

_Review requested from @AlexWaygood by @TomerBin on 2024-10-27 21:30_

---

_Label `red-knot` added by @MichaReiser on 2024-10-28 07:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not.md`:3 on 2024-10-28 20:21_

```suggestion
The `not` operator negates a constraint.
```

---

_@carljm approved on 2024-10-28 20:22_

This is awesome, thank you!!

---

_Comment by @carljm on 2024-10-28 20:24_

> I added a new mdtest file for `not` expression, but maybe we should spread it over the other test files (by adding `not` cases to each one of them)? I'll be happy to hear your preference around that :)

I like what you've done here. I think in general any feature that makes sense to make a PR to add, probably makes sense to have a dedicated file to test as well. Which of course isn't incompatible with perhaps _also_ having it occasionally show up in tests of other features, too, but not in a "must add it everywhere" way.

---

_Merged by @carljm on 2024-10-28 20:27_

---

_Closed by @carljm on 2024-10-28 20:27_

---

_Branch deleted on 2024-10-28 20:49_

---
