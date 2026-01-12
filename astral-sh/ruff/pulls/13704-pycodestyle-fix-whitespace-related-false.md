```yaml
number: 13704
title: "[`pycodestyle`] Fix whitespace-related false positives and false negatives inside type-parameter lists"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - rule
  - python313
assignees: []
merged: true
base: main
head: e251-tvar-default
created_at: 2024-10-10T14:11:59Z
updated_at: 2024-10-10T16:24:20Z
url: https://github.com/astral-sh/ruff/pull/13704
synced_at: 2026-01-12T15:55:45Z
```

# [`pycodestyle`] Fix whitespace-related false positives and false negatives inside type-parameter lists

---

_@AlexWaygood_

## Summary

Fixes #13699.

We had false positives and false negatives regarding whitespace in type-parameter lists, that led to conflicts between the linter and the formatter. Specifically, we emitted a false positive here:

```py
def foo[T = int](): ...  # E251: Unexpected spaces around keyword / parameter equals
```

and a false negative here:

```py
def foo[T:int](): ...  # should be an E231 error here (no space in between `T:` and `int`), but there isn't
```

This PR adds a `TypeParamsState` enum, to keep track of whether we're currently visiting the `LogicalLineToken`s in a type parameters list or not. The common solution is then used to fix the bugs in both rules.

Type parameter lists are new syntax that is only available in Python 3.13+, which is why this is only coming up now.

## Test Plan

Fixtures have been added with examples of both class and function definitions that have type parameters


---

_Label `bug` added by @AlexWaygood on 2024-10-10 14:11_

---

_Label `rule` added by @AlexWaygood on 2024-10-10 14:11_

---

_Label `python313` added by @AlexWaygood on 2024-10-10 14:11_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/mod.rs`:504 on 2024-10-10 14:22_

nit: I think `impl Default` might be suitable instead of an empty `new` method

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/missing_whitespace.rs`:45 on 2024-10-10 14:28_

I find the name confusing because the variable and type name don't match. Should this be `type_params_state`?

---

_@MichaReiser reviewed on 2024-10-10 14:28_

---

_@AlexWaygood reviewed on 2024-10-10 14:28_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/missing_whitespace.rs`:45 on 2024-10-10 14:28_

oops, thanks! In an early version I called the enum something different, and forgot to change this variable name when I changed the name of the enum.

---

_Comment by @github-actions[bot] on 2024-10-10 14:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/missing_whitespace.rs`:45 on 2024-10-10 14:29_

Fixed

---

_@AlexWaygood reviewed on 2024-10-10 14:29_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/whitespace_around_named_parameter_equals.rs`:112 on 2024-10-10 14:34_

I wonder if we could optimize this such that we avoid maintaining the `TypeParamsState` if this logical line does not represent a function / class definition. We could utilize the `in_def` variable and define a `in_class` variable similarly. This is because most logical lines aren't function / class definition. This would also mean that we could remove the `BeforeClassOrDefKeyword` state.

---

_@dhruvmanila reviewed on 2024-10-10 14:34_

---

_@dhruvmanila approved on 2024-10-10 14:44_

Thanks for fixing this! I leave the decision on whether to implement the optimization or not to you if it's feasible before the 0.7 release otherwise that can be deferred for later. I don't see any regression so it might be ok.

---

_@AlexWaygood reviewed on 2024-10-10 16:15_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/whitespace_around_named_parameter_equals.rs`:112 on 2024-10-10 16:15_

Thanks, great suggestion. I added this optimisation in https://github.com/astral-sh/ruff/pull/13704/commits/255ae25cbd673298d5be3cbf41dd316c70249302. Making this change also made me realise that there was a bug in my handling of indented functions or classes :-)

---

_Merged by @AlexWaygood on 2024-10-10 16:24_

---

_Closed by @AlexWaygood on 2024-10-10 16:24_

---

_Branch deleted on 2024-10-10 16:24_

---
