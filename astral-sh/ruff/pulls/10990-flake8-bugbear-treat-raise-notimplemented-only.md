```yaml
number: 10990
title: "[`flake8-bugbear`] Treat `raise NotImplemented`-only bodies as stub functions"
type: pull_request
state: merged
author: Philipp-Thiel
labels:
  - rule
assignees: []
merged: true
base: main
head: B006_stubs
created_at: 2024-04-17T09:02:52Z
updated_at: 2024-04-19T05:06:44Z
url: https://github.com/astral-sh/ruff/pull/10990
synced_at: 2026-01-12T15:55:34Z
```

# [`flake8-bugbear`] Treat `raise NotImplemented`-only bodies as stub functions

---

_@Philipp-Thiel_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

As discussed in https://github.com/astral-sh/ruff/issues/10083#issuecomment-1969653610, stubs detection now also covers the case where the function body raises NotImplementedError and does nothing else.

## Test Plan

Tests for the relevant cases were added in B006_8.py

---

_Comment by @github-actions[bot] on 2024-04-17 09:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-17 13:49_

---

_@charliermarsh approved on 2024-04-17 13:58_

Thanks!

---

_Label `rule` added by @charliermarsh on 2024-04-17 13:58_

---

_@charliermarsh reviewed on 2024-04-17 13:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:237 on 2024-04-17 13:59_

Changed this to use the semantic model, and allow `raise NotImplementedError` (without argument) and `raise NotImplemented`.

---

_Renamed from "B006 stubs extension to include NotImplementedError" to "[`flake8-bugbear`] Treat `raise NotImplemented`-only bodies as stub functions" by @charliermarsh on 2024-04-17 13:59_

---

_Merged by @charliermarsh on 2024-04-17 14:06_

---

_Closed by @charliermarsh on 2024-04-17 14:06_

---

_@NeilGirdhar reviewed on 2024-04-19 02:30_

---

_Review comment by @NeilGirdhar on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:237 on 2024-04-19 02:30_

Why do you allow `raise NotImplemented`?  `NotImplemented` is not an exception.  It should never be raised.  `NotImplemented` can be returned by comparison operators.

---

_@NeilGirdhar reviewed on 2024-04-19 02:31_

---

_Review comment by @NeilGirdhar on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B006_8.py`:20 on 2024-04-19 02:31_

What's the purpose of this example?

---

_@charliermarsh reviewed on 2024-04-19 02:56_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:237 on 2024-04-19 02:56_

We have separate rules to guard against `raise NotImplemented`. But if the entire function is `raise NotImplemented`, it's almost certainly intended to be `raise NotImplementedError`, right?

---

_@charliermarsh reviewed on 2024-04-19 02:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B006_8.py`:20 on 2024-04-19 02:57_

To verify that we treat `raise NotImplemented` like `raise NotImplementedError`. (Just to be clear, these aren't examples, they're tests.)

---

_@NeilGirdhar reviewed on 2024-04-19 05:06_

---

_Review comment by @NeilGirdhar on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:237 on 2024-04-19 05:06_

I see, fair enough!

---
