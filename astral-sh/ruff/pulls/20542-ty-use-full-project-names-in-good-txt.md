```yaml
number: 20542
title: "[ty] Use full project names in `good.txt`"
type: pull_request
state: open
author: sharkdp
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: david/full-project-names
created_at: 2025-09-24T07:10:30Z
updated_at: 2025-09-24T08:56:23Z
url: https://github.com/astral-sh/ruff/pull/20542
synced_at: 2026-01-12T15:57:04Z
```

# [ty] Use full project names in `good.txt`

---

_@sharkdp_

## Summary

This allows `ecosystem-analyzer` to pick up these projects as well (uses full names instead of regex patterns).

## Test Plan

- [ ] Successful run of mypy_primer
- [ ] Made sure that mypy_primer still runs all three CPython projects
- [ ] Successful run of ecosystem-analyzer
- [ ] Made sure that ecossystem-analyzer now picks up the CPython projects

---

_Label `ci` added by @sharkdp on 2025-09-24 07:10_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-24 07:10_

---

_@sharkdp reviewed on 2025-09-24 07:11_

---

_Review comment by @sharkdp on `.github/workflows/ty-ecosystem-report.yaml`:52 on 2025-09-24 07:11_

I only bumped it in `ty-ecosystem-analyzer.yaml`, forgot to bump it here as well.

---

_Comment by @github-actions[bot] on 2025-09-24 07:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-24 07:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-24 07:23_

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

_Label `ty` added by @AlexWaygood on 2025-09-24 08:56_

---
