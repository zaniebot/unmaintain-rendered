```yaml
number: 18313
title: "[ty] Fix attribute writes to unions/intersections including modules"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-module-attribute-writes
created_at: 2025-05-26T09:16:28Z
updated_at: 2025-05-26T09:41:05Z
url: https://github.com/astral-sh/ruff/pull/18313
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Fix attribute writes to unions/intersections including modules

---

_Pull request opened by @sharkdp on 2025-05-26 09:16_

## Summary

Fix a bug that involved writes to attributes on union/intersection types that included modules as elements.

This is a prerequisite to avoid some ecosystem false positives in https://github.com/astral-sh/ruff/pull/18312

## Test Plan

Added regression test

---

_Review requested from @carljm by @sharkdp on 2025-05-26 09:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-26 09:16_

---

_Review requested from @dcreager by @sharkdp on 2025-05-26 09:16_

---

_Label `ty` added by @sharkdp on 2025-05-26 09:16_

---

_Closed by @sharkdp on 2025-05-26 09:18_

---

_Reopened by @sharkdp on 2025-05-26 09:18_

---

_Renamed from "[tx] Fix attribute writes to unions/intersections including modules" to "[ty] Fix attribute writes to unions/intersections including modules" by @MichaReiser on 2025-05-26 09:24_

---

_Comment by @github-actions[bot] on 2025-05-26 09:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dragonchain (https://github.com/dragonchain/dragonchain)
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:58:9: Implicit shadowing of function `get`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:62:9: Implicit shadowing of function `get`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:70:9: Implicit shadowing of function `get`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:78:9: Implicit shadowing of function `put`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:90:9: Implicit shadowing of function `delete`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:98:9: Implicit shadowing of function `list_objects`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:103:9: Implicit shadowing of function `list_objects`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:111:9: Implicit shadowing of function `does_superkey_exist`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:119:9: Implicit shadowing of function `does_object_exist`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:158:9: Implicit shadowing of function `select_transaction`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:163:9: Implicit shadowing of function `select_transaction`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:169:9: Implicit shadowing of function `select_transaction`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:174:9: Implicit shadowing of function `select_transaction`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:178:9: Implicit shadowing of function `select_transaction`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:183:9: Implicit shadowing of function `select_transaction`
- error[invalid-assignment] dragonchain/lib/interfaces/storage_utest.py:187:9: Implicit shadowing of function `select_transaction`
- Found 339 diagnostics
+ Found 323 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-assignment] pwndbg/dbg/lldb/repl/readline.py:24:9: Implicit shadowing of function `set_completion_display_matches_hook`
- error[invalid-assignment] pwndbg/exception.py:118:1: Object of type `@Todo(specialized non-generic class)` is not assignable to attribute `set_trace` on type `Unknown | <module 'pdb'>`
- Found 2249 diagnostics
+ Found 2247 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-assignment] benchmarks/core_api/scenario.py:9:5: Object of type `@Todo(map_with_boundness: intersections with negative contributions)` is not assignable to attribute `dispatch_with_results` on type `<module 'ddtrace.internal.core'> & ~<Protocol with members 'dispatch_with_results'>`
- error[invalid-assignment] ddtrace/vendor/psutil/__init__.py:354:1: Object of type `<class 'NoSuchProcess'>` is not assignable to attribute `NoSuchProcess` on type `<module 'ddtrace.vendor.psutil._pslinux'> | <module 'ddtrace.vendor.psutil._pswindows'> | <module 'ddtrace.vendor.psutil._psosx'> | <module 'ddtrace.vendor.psutil._psbsd'> | <module 'ddtrace.vendor.psutil._pssunos'> | <module 'ddtrace.vendor.psutil._psaix'>`
- error[invalid-assignment] ddtrace/vendor/psutil/__init__.py:355:1: Object of type `<class 'ZombieProcess'>` is not assignable to attribute `ZombieProcess` on type `<module 'ddtrace.vendor.psutil._pslinux'> | <module 'ddtrace.vendor.psutil._pswindows'> | <module 'ddtrace.vendor.psutil._psosx'> | <module 'ddtrace.vendor.psutil._psbsd'> | <module 'ddtrace.vendor.psutil._pssunos'> | <module 'ddtrace.vendor.psutil._psaix'>`
- error[invalid-assignment] ddtrace/vendor/psutil/__init__.py:356:1: Object of type `<class 'AccessDenied'>` is not assignable to attribute `AccessDenied` on type `<module 'ddtrace.vendor.psutil._pslinux'> | <module 'ddtrace.vendor.psutil._pswindows'> | <module 'ddtrace.vendor.psutil._psosx'> | <module 'ddtrace.vendor.psutil._psbsd'> | <module 'ddtrace.vendor.psutil._pssunos'> | <module 'ddtrace.vendor.psutil._psaix'>`
- error[invalid-assignment] ddtrace/vendor/psutil/__init__.py:357:1: Object of type `<class 'TimeoutExpired'>` is not assignable to attribute `TimeoutExpired` on type `<module 'ddtrace.vendor.psutil._pslinux'> | <module 'ddtrace.vendor.psutil._pswindows'> | <module 'ddtrace.vendor.psutil._psosx'> | <module 'ddtrace.vendor.psutil._psbsd'> | <module 'ddtrace.vendor.psutil._pssunos'> | <module 'ddtrace.vendor.psutil._psaix'>`
- Found 6848 diagnostics
+ Found 6843 diagnostics

```
</details>


---

_Comment by @sharkdp on 2025-05-26 09:41_

The ecosystem results all look good. Merging this, as it seems uncontroversial.

---

_Merged by @sharkdp on 2025-05-26 09:41_

---

_Closed by @sharkdp on 2025-05-26 09:41_

---

_Branch deleted on 2025-05-26 09:41_

---
