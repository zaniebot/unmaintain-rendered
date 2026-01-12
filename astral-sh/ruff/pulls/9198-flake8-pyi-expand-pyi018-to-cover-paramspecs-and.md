```yaml
number: 9198
title: "[flake8-pyi] Expand PYI018 to cover ParamSpecs and TypeVarTuples"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: pyi018-false-negs
created_at: 2023-12-19T12:04:47Z
updated_at: 2023-12-20T07:47:43Z
url: https://github.com/astral-sh/ruff/pull/9198
synced_at: 2026-01-12T15:55:28Z
```

# [flake8-pyi] Expand PYI018 to cover ParamSpecs and TypeVarTuples

---

_@AlexWaygood_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Part of #8771. flake8-pyi will emit a Y018 error for unused TypeVars, ParamSpecs or TypeVarTuples; Ruff currently only emits PYI018 for unused TypeVars.

This is my first "proper" Ruff PR -- let me know if there's a better way of doing this! Not sure if the repeated calls to `match_typing_expr()` are ideal.

## Test Plan

I manually updated the fixtures to add some unused ParamSpecs and TypeVarTuples, and then updated the snapshots using `cargo insta review`. All tests then passed when run using `cargo test`.


---

_Renamed from "Pyi018 false negs" to "Expand PYI018 to cover ParamSpecs and TypeVarTuples" by @AlexWaygood on 2023-12-19 12:05_

---

_Renamed from "Expand PYI018 to cover ParamSpecs and TypeVarTuples" to "[flake8-pyi] Expand PYI018 to cover ParamSpecs and TypeVarTuples" by @AlexWaygood on 2023-12-19 12:08_

---

_Comment by @github-actions[bot] on 2023-12-19 12:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @charliermarsh on 2023-12-19 21:13_

---

_@charliermarsh reviewed on 2023-12-20 02:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unused_private_type_definition.rs`:209 on 2023-12-20 02:45_

I might write this like:
```rust
let typevarlike_kind = if semantic.match_typing_expr(f, "TypeVar")  {
   "TypeVar"
} else if ...
```

But I don't know that what I'm proposing is actually _better_, I just hadn't seen this pattern before :)

---

_@charliermarsh approved on 2023-12-20 03:01_

Great, thank you!

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unused_private_type_definition.rs`:202 on 2023-12-20 03:05_

@AlexWaygood - I made one change here per your comment in the summary. Each call to `semantic.match_typing_expr` does a lookup internally to map the expression to a "call path", like `["typing", "TypeVar"]`. However, we can just do that lookup once via `semantic.resolve_call_path`, then pass the call path to `semantic.match_typing_call_path`. (`semantic.match_typing_expr` is just a wrapper around this process, so if we want to do multiple checks, it's more efficient to do the lookup outside of the `semantic.match_typing_expr`.)


---

_@charliermarsh reviewed on 2023-12-20 03:05_

---

_Merged by @charliermarsh on 2023-12-20 03:10_

---

_Closed by @charliermarsh on 2023-12-20 03:10_

---

_Branch deleted on 2023-12-20 07:43_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/unused_private_type_definition.rs`:202 on 2023-12-20 07:47_

Ahh nice, thanks! Yep, that sounds like it's exactly what I was looking for :)

---

_@AlexWaygood reviewed on 2023-12-20 07:47_

---
