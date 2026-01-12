```yaml
number: 21028
title: "[`ruff`] Autogenerate TypeParam nodes"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: autogenerate-type-param-nodes
created_at: 2025-10-22T11:33:12Z
updated_at: 2025-10-22T16:25:18Z
url: https://github.com/astral-sh/ruff/pull/21028
synced_at: 2026-01-12T15:57:14Z
```

# [`ruff`] Autogenerate TypeParam nodes

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes a part of #15655

Auto generate ast TypeParam nodes

## Test Plan

<!-- How was it tested? -->

I have fixed some syntax tests. All other tests should pass without any changes. I have confirmed the generated code to make sure it's the same as old code.

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-10-22 11:33_

---

_Review requested from @dhruvmanila by @TaKO8Ki on 2025-10-22 11:33_

---

_@MichaReiser approved on 2025-10-22 11:41_

Thank you

---

_Label `internal` added by @MichaReiser on 2025-10-22 11:42_

---

_Label `parser` added by @MichaReiser on 2025-10-22 11:42_

---

_Comment by @github-actions[bot] on 2025-10-22 11:50_

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

_Merged by @MichaReiser on 2025-10-22 12:06_

---

_Closed by @MichaReiser on 2025-10-22 12:06_

---

_Branch deleted on 2025-10-22 16:25_

---
