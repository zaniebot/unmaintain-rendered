```yaml
number: 19991
title: "[ty] Fix server hang"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/salsa-runaway
created_at: 2025-08-19T16:55:48Z
updated_at: 2025-08-20T08:28:32Z
url: https://github.com/astral-sh/ruff/pull/19991
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix server hang

---

_Pull request opened by @MichaReiser on 2025-08-19 16:55_

## Summary

This PR updates salsa to pull in https://github.com/salsa-rs/salsa/pull/981

This fixes astral-sh/ty#1024

## Test Plan




---

_Label `bug` added by @MichaReiser on 2025-08-19 16:55_

---

_Label `server` added by @MichaReiser on 2025-08-19 16:55_

---

_Label `ty` added by @MichaReiser on 2025-08-19 16:55_

---

_Comment by @github-actions[bot] on 2025-08-19 16:57_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-20 08:08:37.212689668 +0000
+++ new-output.txt	2025-08-20 08:08:39.694685132 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(9637)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(bc7d)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-19 17:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~273MB
+ TOTAL MEMORY USAGE: ~260MB
-     memo fields = ~204MB
+     memo fields = ~194MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-08-19 17:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsalsa-runaway?runnerMode=Instrumentation)

### Merging #19991 will **not alter performance**

<sub>Comparing <code>micha/salsa-runaway</code> (ecc9a97) with <code>main</code> (f019cfd)</sub>



### Summary

`✅ 42` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-08-19 17:11_

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

_Comment by @MichaReiser on 2025-08-19 17:15_

Hmm, not what I expected to see but also not entirely surprising, given that we now need to collect more information. 

---

_Comment by @MichaReiser on 2025-08-19 17:34_

Okay, so pulling in the tracked struct fix results in a memory decrease, which is great. But pulling in the incrementality fix shows a regression on the incremental benchmark (which isn't entirely surprising but still a little surprising)

---

_Marked ready for review by @MichaReiser on 2025-08-20 08:04_

---

_Merged by @MichaReiser on 2025-08-20 08:28_

---

_Closed by @MichaReiser on 2025-08-20 08:28_

---

_Branch deleted on 2025-08-20 08:28_

---
