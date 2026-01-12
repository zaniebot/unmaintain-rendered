```yaml
number: 18927
title: "[`flake8-annotations`] Do not automatically set the return type to `NoReturn` when raising `NotImplementedError`"
type: pull_request
state: closed
author: danparizher
labels: []
assignees: []
base: main
head: fix-18886
created_at: 2025-06-24T23:36:19Z
updated_at: 2025-11-07T04:43:35Z
url: https://github.com/astral-sh/ruff/pull/18927
synced_at: 2026-01-12T15:56:28Z
```

# [`flake8-annotations`] Do not automatically set the return type to `NoReturn` when raising `NotImplementedError`

---

_@danparizher_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #18886

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "fix-18886" to "[`ANN`] automatically sets the return type to NoReturn to raise NotImplementedError" by @danparizher on 2025-06-24 23:36_

---

_Renamed from "[`ANN`] automatically sets the return type to NoReturn to raise NotImplementedError" to "[`flake8-annotations`] automatically sets the return type to NoReturn to raise NotImplementedError" by @danparizher on 2025-06-24 23:37_

---

_Renamed from "[`flake8-annotations`] automatically sets the return type to NoReturn to raise NotImplementedError" to "[`flake8-annotations`] Do not automatically set the return type to `NoReturn` when raising `NotImplementedError`" by @danparizher on 2025-06-24 23:41_

---

_Comment by @github-actions[bot] on 2025-06-24 23:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:236 on 2025-06-25 09:23_

It seems unfortunate that we need to traverse the entire function again to determine if all of them were `RaiseNotImplemented`. Should we add a new `Terminal::RaiseNotImplemented` variant instead?

---

_@MichaReiser reviewed on 2025-06-25 09:23_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:236 on 2025-06-25 13:43_

Good point—but the extra walk only happens when we’ve already determined the function is `Terminal::Raise`, so the cost is minimal. Extending `Terminal` with a `RaiseNotImplemented` would mix exception-specific semantics into what’s currently a pure control-flow enum and force every existing caller to handle a new case.

---

_@danparizher reviewed on 2025-06-25 13:43_

---

_Comment by @danparizher on 2025-11-07 04:42_

Going to close and redo this PR as it has been stale for a few months

---

_Closed by @danparizher on 2025-11-07 04:43_

---

_Branch deleted on 2025-11-07 04:43_

---
