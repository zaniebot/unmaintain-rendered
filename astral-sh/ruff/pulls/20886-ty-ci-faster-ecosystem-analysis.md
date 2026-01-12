```yaml
number: 20886
title: "[ty] CI: Faster ecosystem analysis"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/faster-ecosystem-runs
created_at: 2025-10-15T09:58:35Z
updated_at: 2025-10-15T10:38:19Z
url: https://github.com/astral-sh/ruff/pull/20886
synced_at: 2026-01-12T15:57:11Z
```

# [ty] CI: Faster ecosystem analysis

---

_@sharkdp_

## Summary

I considered making a dedicated cargo profile for these, but the `profiling` profile basically made all the modifications to `release` that I would have also made.

## Test Plan

CI on this PR

---

_Label `ci` added by @sharkdp on 2025-10-15 09:58_

---

_Label `ty` added by @sharkdp on 2025-10-15 09:58_

---

_@sharkdp reviewed on 2025-10-15 09:58_

---

_Review comment by @sharkdp on `scripts/mypy_primer.sh`:23 on 2025-10-15 09:58_

Needs to be updated after the upstream PR is merged

---

_Comment by @github-actions[bot] on 2025-10-15 10:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 51 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-15 10:05_

---

_Comment by @github-actions[bot] on 2025-10-15 10:08_

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

_Comment by @github-actions[bot] on 2025-10-15 10:10_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-faster-ecosystem-runs.ecosystem-663.pages.dev/diff)** ([timing results](https://david-faster-ecosystem-runs.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-15 10:23_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-15 10:23_

---

_@MichaReiser approved on 2025-10-15 10:32_

---

_Marked ready for review by @sharkdp on 2025-10-15 10:38_

---

_Merged by @sharkdp on 2025-10-15 10:38_

---

_Closed by @sharkdp on 2025-10-15 10:38_

---

_Branch deleted on 2025-10-15 10:38_

---
