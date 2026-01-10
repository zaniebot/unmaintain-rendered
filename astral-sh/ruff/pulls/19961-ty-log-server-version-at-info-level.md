```yaml
number: 19961
title: "[ty] Log server version at info level"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha-server-version
created_at: 2025-08-18T07:13:18Z
updated_at: 2025-08-18T07:16:54Z
url: https://github.com/astral-sh/ruff/pull/19961
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Log server version at info level

---

_Pull request opened by @MichaReiser on 2025-08-18 07:13_

## Summary

Log the server version at level "info".

It's not always clear for users what ty version the extension uses. 
That's why it's useful to log that information without the need to change the log level.




---

_Review requested from @carljm by @MichaReiser on 2025-08-18 07:13_

---

_Label `server` added by @MichaReiser on 2025-08-18 07:13_

---

_Label `ty` added by @MichaReiser on 2025-08-18 07:13_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-18 07:13_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-18 07:13_

---

_Comment by @github-actions[bot] on 2025-08-18 07:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-18 07:14:50.018682216 +0000
+++ new-output.txt	2025-08-18 07:14:50.090682320 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1b036)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(fcc0)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Merged by @MichaReiser on 2025-08-18 07:16_

---

_Closed by @MichaReiser on 2025-08-18 07:16_

---

_Branch deleted on 2025-08-18 07:16_

---
