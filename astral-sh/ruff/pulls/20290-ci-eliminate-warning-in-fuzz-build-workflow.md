```yaml
number: 20290
title: "CI: Eliminate warning in fuzz build workflow"
type: pull_request
state: merged
author: ferdnyc
labels:
  - ci
assignees: []
merged: true
base: main
head: fix-fuzz-ci-warning
created_at: 2025-09-07T22:53:20Z
updated_at: 2025-09-08T06:13:36Z
url: https://github.com/astral-sh/ruff/pull/20290
synced_at: 2026-01-12T15:56:58Z
```

# CI: Eliminate warning in fuzz build workflow

---

_@ferdnyc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Pr #11919 changed the fuzz build from `taiki-e/install-action` to `cargo-bins/cargo-binstall` for necessary reasons of version selection. But it left the `with:` parameter, which the `binstall` action does not support. As a result, all workflow runs are showing a warning:
> Unexpected input(s) `'tool'`, valid inputs are `['']`

Eliminate the warning by removing the `with` parameter.

## Test Plan

Run CI, determine that the "cargo fuzz build" step no longer includes an Annotation showing the warning message (quoted above).


---

_Label `internal` added by @dhruvmanila on 2025-09-08 05:36_

---

_Label `ci` added by @dhruvmanila on 2025-09-08 05:36_

---

_Label `internal` removed by @dhruvmanila on 2025-09-08 05:36_

---

_@dhruvmanila approved on 2025-09-08 06:03_

Thank you!

---

_Comment by @github-actions[bot] on 2025-09-08 06:03_

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

_Merged by @dhruvmanila on 2025-09-08 06:03_

---

_Closed by @dhruvmanila on 2025-09-08 06:03_

---

_Branch deleted on 2025-09-08 06:13_

---
