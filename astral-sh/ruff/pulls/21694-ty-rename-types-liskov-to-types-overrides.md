```yaml
number: 21694
title: "[ty] Rename `types::liskov` to `types::overrides`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/rename-liskov-module
created_at: 2025-11-29T13:13:42Z
updated_at: 2025-11-29T14:54:02Z
url: https://github.com/astral-sh/ruff/pull/21694
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Rename `types::liskov` to `types::overrides`

---

_Pull request opened by @AlexWaygood on 2025-11-29 13:13_

## Summary

This module now also contains the logic for the `invalid-explicit-override` rule and the `override-of-final-method` rule; it's no longer just about Liskov violations.

## Test Plan

All existing tests pass


---

_Review requested from @carljm by @AlexWaygood on 2025-11-29 13:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-29 13:13_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-29 13:13_

---

_Label `internal` added by @AlexWaygood on 2025-11-29 13:13_

---

_Label `ty` added by @AlexWaygood on 2025-11-29 13:13_

---

_Comment by @astral-sh-bot[bot] on 2025-11-29 13:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-29 13:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 496 diagnostics
+ Found 498 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser approved on 2025-11-29 14:43_

---

_Merged by @AlexWaygood on 2025-11-29 14:54_

---

_Closed by @AlexWaygood on 2025-11-29 14:54_

---

_Branch deleted on 2025-11-29 14:54_

---
