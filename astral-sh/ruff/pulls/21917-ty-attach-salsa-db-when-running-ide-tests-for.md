```yaml
number: 21917
title: "[ty] Attach salsa db when running ide tests for easier debugging"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/salsa-debugging-ide-tests
created_at: 2025-12-11T15:05:51Z
updated_at: 2025-12-11T18:03:54Z
url: https://github.com/astral-sh/ruff/pull/21917
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Attach salsa db when running ide tests for easier debugging

---

_Pull request opened by @MichaReiser on 2025-12-11 15:05_

## Summary

`dbg(salsa_struct)` only prints the salsa ID when called outside a query because 
the Salsa db isn't set for the current thread. 

This PR changes a few IDE tests to always attach the Salsa DB to the current thread
when calling the functions under tests so that debug printing salsa structs prints
the entire structs and not just the ID, making them much more useful.



---

_Review requested from @carljm by @MichaReiser on 2025-12-11 15:05_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-11 15:05_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-11 15:05_

---

_Label `testing` added by @MichaReiser on 2025-12-11 15:05_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-11 15:05_

---

_Label `ty` added by @MichaReiser on 2025-12-11 15:05_

---

_@dcreager approved on 2025-12-11 15:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 15:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 15:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5123 diagnostics
+ Found 5122 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-11 16:31_

---

_Merged by @MichaReiser on 2025-12-11 18:03_

---

_Closed by @MichaReiser on 2025-12-11 18:03_

---

_Branch deleted on 2025-12-11 18:03_

---
