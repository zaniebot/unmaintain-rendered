```yaml
number: 18840
title: "[`flake8-logging`] Add fix safety section to `LOG002`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels: []
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-06-20T23:18:05Z
updated_at: 2025-06-23T16:57:06Z
url: https://github.com/astral-sh/ruff/pull/18840
synced_at: 2026-01-12T15:56:26Z
```

# [`flake8-logging`] Add fix safety section to `LOG002`

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #15584

This adds a `Fix safety` section to [invalid-get-logger-argument (LOG002)](https://docs.astral.sh/ruff/rules/invalid-get-logger-argument/#invalid-get-logger-argument-log002).

The fix/lint was introduced in #7399
No reasoning is given on the unsafety in the PR/code
Unsafe fix demonstration:
[playground](https://play.ruff.rs/e8008cbf-2ef5-4d38-8255-324f90e624cb)
```py
import logging
logger = logging.getLogger(__file__)
```

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected

---

_Comment by @github-actions[bot] on 2025-06-20 23:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-21 15:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_logging/rules/invalid_get_logger_argument.rs`:43 on 2025-06-23 13:19_

```suggestion
/// This fix is always unsafe, as changing the arguments to `getLogger` can change the
```

---

_@dylwil3 approved on 2025-06-23 13:19_

Thank you!

---

_Merged by @dylwil3 on 2025-06-23 13:23_

---

_Closed by @dylwil3 on 2025-06-23 13:23_

---

_Branch deleted on 2025-06-23 16:57_

---
