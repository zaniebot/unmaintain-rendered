```yaml
number: 20412
title: "[ty] Remove 'materialize' from the ecosystem projects"
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
head: david/remove-materialize
created_at: 2025-09-15T08:17:00Z
updated_at: 2025-09-15T08:42:37Z
url: https://github.com/astral-sh/ruff/pull/20412
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Remove 'materialize' from the ecosystem projects

---

_Pull request opened by @sharkdp on 2025-09-15 08:17_

## Summary

This project was [recently removed from mypy_primer](https://github.com/astral-sh/ruff/pull/20378), so we need to remove it from `good.txt` in order for ecosystem-analyzer to work correctly.

## Test Plan

Run mypy_primer and ecosystem-analyzer on this branch.


---

_Review requested from @carljm by @sharkdp on 2025-09-15 08:17_

---

_Label `ci` added by @sharkdp on 2025-09-15 08:17_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-15 08:17_

---

_Review requested from @dcreager by @sharkdp on 2025-09-15 08:17_

---

_Label `ty` added by @sharkdp on 2025-09-15 08:17_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-15 08:18_

---

_Comment by @github-actions[bot] on 2025-09-15 08:19_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-15 08:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-15 08:27_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-15 08:28_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-15 08:34_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-15 08:34_

---

_Comment by @github-actions[bot] on 2025-09-15 08:38_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-remove-materialize.ecosystem-663.pages.dev/diff)**


---

_Merged by @sharkdp on 2025-09-15 08:42_

---

_Closed by @sharkdp on 2025-09-15 08:42_

---

_Branch deleted on 2025-09-15 08:42_

---
