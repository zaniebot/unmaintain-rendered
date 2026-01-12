```yaml
number: 21727
title: "[ty] Show the user where the type variable was defined in `invalid-type-arguments` diagnostics"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/argument-types-debug
created_at: 2025-12-01T12:13:31Z
updated_at: 2025-12-01T12:25:51Z
url: https://github.com/astral-sh/ruff/pull/21727
synced_at: 2026-01-12T15:57:31Z
```

# [ty] Show the user where the type variable was defined in `invalid-type-arguments` diagnostics

---

_@AlexWaygood_

## Summary

It's hard to debug the false-positive diagnostics in https://github.com/astral-sh/ty/issues/1685 because the diagnostics complain that the argument type is invalid for a certain type variable, but doesn't show you where the type variable is defined. This PR adds secondary annotations so the user can jump straight there and debug the diagnostic more easily.

## Test Plan

added snapshots.


---

_Review requested from @carljm by @AlexWaygood on 2025-12-01 12:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-01 12:13_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-01 12:13_

---

_Label `ty` added by @AlexWaygood on 2025-12-01 12:13_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-01 12:13_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:18_


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

_@sharkdp approved on 2025-12-01 12:20_

Nice!

---

_Merged by @AlexWaygood on 2025-12-01 12:25_

---

_Closed by @AlexWaygood on 2025-12-01 12:25_

---

_Branch deleted on 2025-12-01 12:25_

---
