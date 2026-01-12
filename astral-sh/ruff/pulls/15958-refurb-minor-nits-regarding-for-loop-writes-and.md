```yaml
number: 15958
title: "[`refurb`] Minor nits regarding `for-loop-writes` and `for-loop-set-mutations`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/refurb-nits
created_at: 2025-02-05T10:09:57Z
updated_at: 2025-02-05T10:21:38Z
url: https://github.com/astral-sh/ruff/pull/15958
synced_at: 2026-01-12T15:55:53Z
```

# [`refurb`] Minor nits regarding `for-loop-writes` and `for-loop-set-mutations`

---

_@AlexWaygood_

## Summary

Minor nit followups to #15953:
- Move the `parenthesize_loop_iter_if_necessary` function to a `helpers` module, so that one rule doesn't have to import a function from the other rule. That feels like an antipattern; rules should generally be ~isolated from each other.
- Add docs to the `parenthesize_loop_iter_if_necessary` function
- Use a `Cow` for the return type of `parenthesize_loop_iter_if_necessary` to avoid an unnecessary allocation
- Document why the fix for `for-loop-set-mutations` is now sometimes marked as unsafe.

## Test Plan

`cargo test -p ruff_linter`


---

_Label `internal` added by @AlexWaygood on 2025-02-05 10:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/helpers.rs`:17 on 2025-02-05 10:16_

Using a `Cow` here is nice but it feels a bit overkill, considering that this is a fix. 

---

_@MichaReiser approved on 2025-02-05 10:16_

> Move the parenthesize_loop_iter_if_necessary function to a helpers module, so that one rule doesn't have to import a function from the other rule. That feels like an antipattern; rules should generally be ~isolated from each other.

I don't mind this too much. I consider `helpers` an anti-pattern because they're just a huge pile of random functions that are impossible to discover (because the file is called `helpers` in every group). Ideally, we'd move those "helpers" to semantically meaningful mdoules.

---

_Comment by @github-actions[bot] on 2025-02-05 10:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-02-05 10:21_

> I consider `helpers` an anti-pattern because they're just a huge pile of random functions that are impossible to discover (because the file is called `helpers` in every group). Ideally, we'd move those "helpers" to semantically meaningful mdoules.

Yeah, I agree. It still feels better than having one rule import from another rule, though (to me, anyway). And probably not worth spending much more time on right now ;)

---

_Merged by @AlexWaygood on 2025-02-05 10:21_

---

_Closed by @AlexWaygood on 2025-02-05 10:21_

---

_Branch deleted on 2025-02-05 10:21_

---
