```yaml
number: 11150
title: "Delete unused methods from `Parameters`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: unused-param-methods
created_at: 2024-04-25T17:06:49Z
updated_at: 2024-04-25T21:11:26Z
url: https://github.com/astral-sh/ruff/pull/11150
synced_at: 2026-01-12T15:55:35Z
```

# Delete unused methods from `Parameters`

---

_@AlexWaygood_

## Summary

I was reading through `nodes.rs`, and wondered what the `Parameters::defaults()` and `Parameters::split_kwonlyargs()` methods were used for (especially the second one, which seemed complex!). Turns out, we don't use them at all -- so this PR gets rid of them.

While I was here, I also:
- Got rid of `ParameterWithDefault::as_parameter()`. This has a single callsite, but it's less code just to use a simple closure at that callsite, so it doesn't seem to be worth a convenience method to me.
- Got rid of `Parameters::empty()`, and derived `Parameters::default()` instead. The only difference between the two is that `Parameters::empty()` currently requires you to pass in a `TextRange` instance -- but we only use the method twice across our codebase currently, and in both instances, we construct it with `Parameters::empty(TextRange::default())`. 

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-04-25 17:06_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-04-25 17:06_

---

_@AlexWaygood reviewed on 2024-04-25 17:08_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/no_self_use.rs`:105 on 2024-04-25 17:08_

This is unrelated to the other changes in this PR, but it's invalid for `self` to be a keyword-only parameter, so I figured I'd fix it while I was here

---

_Comment by @github-actions[bot] on 2024-04-25 17:25_

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

_@MichaReiser approved on 2024-04-25 20:46_

---

_Label `internal` added by @MichaReiser on 2024-04-25 20:46_

---

_Merged by @AlexWaygood on 2024-04-25 21:11_

---

_Closed by @AlexWaygood on 2024-04-25 21:11_

---

_Branch deleted on 2024-04-25 21:11_

---
