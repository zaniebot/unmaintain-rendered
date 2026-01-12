```yaml
number: 13750
title: "[red knot] Use memmem::find instead of custom version"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/infer-bytes-literal-comparisons-follow-up
created_at: 2024-10-14T12:43:42Z
updated_at: 2024-10-14T13:17:21Z
url: https://github.com/astral-sh/ruff/pull/13750
synced_at: 2026-01-12T15:55:45Z
```

# [red knot] Use memmem::find instead of custom version

---

_@sharkdp_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This is a follow-up on #13746:

- Use `memmem::find` instead of rolling our own inferior version.
- Avoid `x.as_ref()` calls using `&**x`

## Test Plan

—

---

_Label `red-knot` added by @sharkdp on 2024-10-14 12:43_

---

_Review requested from @carljm by @sharkdp on 2024-10-14 12:43_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-14 12:43_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-14 12:43_

---

_@MichaReiser approved on 2024-10-14 12:48_

Thx

---

_@AlexWaygood approved on 2024-10-14 12:49_

---

_Comment by @github-actions[bot] on 2024-10-14 13:02_

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

_Merged by @sharkdp on 2024-10-14 13:17_

---

_Closed by @sharkdp on 2024-10-14 13:17_

---

_Branch deleted on 2024-10-14 13:17_

---
