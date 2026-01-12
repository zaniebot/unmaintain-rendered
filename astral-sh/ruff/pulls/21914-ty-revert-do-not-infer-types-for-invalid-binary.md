```yaml
number: 21914
title: "[ty] Revert \"Do not infer types for invalid binary expressions in annotations\""
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/revert-binary-type-expr-inference
created_at: 2025-12-11T11:52:48Z
updated_at: 2025-12-11T11:57:47Z
url: https://github.com/astral-sh/ruff/pull/21914
synced_at: 2026-01-12T15:57:36Z
```

# [ty] Revert "Do not infer types for invalid binary expressions in annotations"

---

_@sharkdp_

See discussion here: https://github.com/astral-sh/ruff/pull/21911#discussion_r2610155157

---

_Review requested from @carljm by @sharkdp on 2025-12-11 11:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-11 11:52_

---

_Review requested from @dcreager by @sharkdp on 2025-12-11 11:52_

---

_Label `ty` added by @sharkdp on 2025-12-11 11:52_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 11:54_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@AlexWaygood approved on 2025-12-11 11:55_

Thank you!

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 11:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5123 diagnostics
+ Found 5122 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `internal` added by @sharkdp on 2025-12-11 11:57_

---

_Merged by @sharkdp on 2025-12-11 11:57_

---

_Closed by @sharkdp on 2025-12-11 11:57_

---

_Branch deleted on 2025-12-11 11:57_

---
