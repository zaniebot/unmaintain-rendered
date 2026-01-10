```yaml
number: 21060
title: "[ty] Delegate truthiness inference of an enum `Literal` type to its enum-instance supertype"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/fix-property-tests
created_at: 2025-10-24T12:53:28Z
updated_at: 2025-10-24T13:34:18Z
url: https://github.com/astral-sh/ruff/pull/21060
synced_at: 2026-01-10T17:28:45Z
```

# [ty] Delegate truthiness inference of an enum `Literal` type to its enum-instance supertype

---

_Pull request opened by @AlexWaygood on 2025-10-24 12:53_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1435

## Test Plan

- Added mdtests
- Fixed existing TODOs in mdtests
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable` and confirmed that the property tests pass again


---

_Comment by @github-actions[bot] on 2025-10-24 12:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-24 12:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---
