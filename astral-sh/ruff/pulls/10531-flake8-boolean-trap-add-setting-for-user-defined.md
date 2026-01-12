```yaml
number: 10531
title: "[`flake8-boolean-trap`] Add setting for user defined allowed boolean trap"
type: pull_request
state: merged
author: augustelalande
labels: []
assignees: []
merged: true
base: main
head: extend-boolean-trap
created_at: 2024-03-23T00:27:37Z
updated_at: 2024-03-31T15:58:00Z
url: https://github.com/astral-sh/ruff/pull/10531
synced_at: 2026-01-12T15:55:32Z
```

# [`flake8-boolean-trap`] Add setting for user defined allowed boolean trap

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add a setting `extend-allowed-calls` to allow users to define their own list of calls which allow boolean traps.

Resolves #10485.
Resolves #10356.

## Test Plan

Extended text fixture and added setting test.


---

_Comment by @github-actions[bot] on 2024-03-23 01:01_

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

_Comment by @9128305 on 2024-03-23 03:48_

Is it possible to also check the import path like it should check "django.db.models.Value" and not just "Value" usage?

---

_@charliermarsh reviewed on 2024-03-25 02:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_boolean_trap/mod.rs`:61 on 2024-03-25 02:29_

It's not clear to me if these should be function names, or symbol paths (like `["pydantic", "BaseModel"]`).

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_boolean_trap/mod.rs`:61 on 2024-03-25 02:54_

Do you mean it's not clear in the documentation/implementation? Or are you saying you're not sure how we should implement it?

---

_@augustelalande reviewed on 2024-03-25 02:54_

---

_@charliermarsh reviewed on 2024-03-25 03:05_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_boolean_trap/mod.rs`:61 on 2024-03-25 03:05_

The latter!

---

_@augustelalande reviewed on 2024-03-25 04:05_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_boolean_trap/mod.rs`:61 on 2024-03-25 04:05_

I think symbol path gives finer control, and is less prone to false negatives. The only downside is that it is less intuitive to users, and may lead to confusion. But I think this should be ok with good documentation.

---

_@augustelalande reviewed on 2024-03-25 04:06_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_boolean_trap/mod.rs`:61 on 2024-03-25 04:06_

I will change it to symbol paths if everyone agrees.

---

_@93578237 reviewed on 2024-03-25 06:45_

---

_Review comment by @93578237 on `crates/ruff_linter/src/rules/flake8_boolean_trap/mod.rs`:61 on 2024-03-25 06:45_

is it hard to support both?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_boolean_trap/mod.rs`:61 on 2024-03-25 14:38_

Symbol path is a little problematic because it doesn't support _methods_ (e.g., there's no way to identify `y` in `x = pydantic.BaseModel(); x.y()`), and methods are a common use-case here.

---

_@charliermarsh reviewed on 2024-03-25 14:38_

---

_Comment by @augustelalande on 2024-03-25 16:18_

@charliermarsh I'm moving the discussion to the main thread.

Given your comment I think you do not want to drop support for simple function calls.

Then there are three options:
1. Keep things as is with users only defining functions to ignore.
2. Implement both function names and symbol paths using a single setting. It is then up to us to determine whether the given pattern should be interpreted as a function or symbol path (I think it's not that hard?).
3. Implement both with two settings, one for symbol path, one for functions.

---

_Comment by @augustelalande on 2024-03-25 16:18_

I lean more towards 2, but not strongly 

---

_Comment by @charliermarsh on 2024-03-28 01:07_

Let me quickly cross-reference with some other settings.

---

_Comment by @charliermarsh on 2024-03-28 01:21_

The two linked examples, at least, seem like they would work with symbol paths. So let's start with that. (`extend_immutable_calls` works with symbol paths, so symbol paths are more consistent with existing settings.)

---

_Comment by @augustelalande on 2024-03-28 03:33_

Done

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-29 19:57_

---

_@charliermarsh approved on 2024-03-30 00:17_

---

_Merged by @charliermarsh on 2024-03-30 00:26_

---

_Closed by @charliermarsh on 2024-03-30 00:26_

---

_Branch deleted on 2024-03-31 15:58_

---
