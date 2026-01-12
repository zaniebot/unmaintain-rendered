```yaml
number: 20206
title: "[ty] Explicit control over cycle entrypoint"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/fix-1111-2
created_at: 2025-09-02T14:01:16Z
updated_at: 2025-09-03T11:18:41Z
url: https://github.com/astral-sh/ruff/pull/20206
synced_at: 2026-01-12T15:56:56Z
```

# [ty] Explicit control over cycle entrypoint

---

_@sharkdp_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-09-02 14:01_

---

_Comment by @github-actions[bot] on 2025-09-02 14:03_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-02 14:03:12.605117900 +0000
+++ new-output.txt	2025-09-02 14:03:15.414128580 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(b8e9)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(c0e9)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-02 14:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~4MB
+     struct fields = ~5MB

trio (https://github.com/python-trio/trio)
-     memo metadata = ~22MB
+     memo metadata = ~23MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct metadata = ~12MB
+     struct metadata = ~14MB
-     struct fields = ~17MB
+     struct fields = ~18MB
-     memo metadata = ~38MB
+     memo metadata = ~40MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~27MB
+     struct metadata = ~30MB
-     memo metadata = ~80MB
+     memo metadata = ~84MB

```
</details>


---

_Closed by @sharkdp on 2025-09-03 11:18_

---
