```yaml
number: 18841
title: "[`flake8-logging`] Add fix safety section to `LOG001`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-3
created_at: 2025-06-20T23:26:24Z
updated_at: 2025-06-23T16:57:03Z
url: https://github.com/astral-sh/ruff/pull/18841
synced_at: 2026-01-12T15:56:26Z
```

# [`flake8-logging`] Add fix safety section to `LOG001`

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

This adds a `Fix safety` section to [direct-logger-instantiation (LOG001)](https://docs.astral.sh/ruff/rules/direct-logger-instantiation/#direct-logger-instantiation-log001).

The fix/lint was introduced in #7397
No reasoning is given on the unsafety in the PR/code
Unsafe fix demonstration:
[playground](https://play.ruff.rs/72e7277e-a9db-4cd9-9afb-2c56ef2db361)
```py
import logging
logger = logging.Logger(__name__)
```

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected

---

_Comment by @github-actions[bot] on 2025-06-20 23:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-21 15:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_logging/rules/direct_logger_instantiation.rs`:41 on 2025-06-23 13:15_

```suggestion
/// changes program behavior by will adding the logger to the logging tree.
```

---

_@dylwil3 approved on 2025-06-23 13:16_

Thanks!

---

_Merged by @dylwil3 on 2025-06-23 13:19_

---

_Closed by @dylwil3 on 2025-06-23 13:19_

---

_Label `documentation` added by @dylwil3 on 2025-06-23 13:23_

---

_Branch deleted on 2025-06-23 16:57_

---
