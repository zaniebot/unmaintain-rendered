```yaml
number: 21689
title: "[ty] fix typo in HasDefinition trait docstring"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: fix-typo-has-definition-trait
created_at: 2025-11-29T09:56:05Z
updated_at: 2025-11-29T11:14:08Z
url: https://github.com/astral-sh/ruff/pull/21689
synced_at: 2026-01-10T16:48:02Z
```

# [ty] fix typo in HasDefinition trait docstring

---

_Pull request opened by @RasmusNygren on 2025-11-29 09:56_

## Summary
Fixes a typo in the docstring for the definition method in the HasDefinition trait

---

_Review requested from @carljm by @RasmusNygren on 2025-11-29 09:56_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-11-29 09:56_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-11-29 09:56_

---

_Review requested from @dcreager by @RasmusNygren on 2025-11-29 09:56_

---

_Comment by @astral-sh-bot[bot] on 2025-11-29 09:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-29 09:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 496 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood approved on 2025-11-29 11:13_

---

_Merged by @AlexWaygood on 2025-11-29 11:13_

---

_Closed by @AlexWaygood on 2025-11-29 11:13_

---

_Label `internal` added by @AlexWaygood on 2025-11-29 11:14_

---

_Label `ty` added by @AlexWaygood on 2025-11-29 11:14_

---
