```yaml
number: 21681
title: "[ty] Fix override of final method summary"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: final-method-override-summary
created_at: 2025-11-28T16:12:54Z
updated_at: 2025-12-27T00:21:15Z
url: https://github.com/astral-sh/ruff/pull/21681
synced_at: 2026-01-12T15:57:30Z
```

# [ty] Fix override of final method summary

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Think this was just copied over incorrectly in #21646.


---

_Review requested from @carljm by @MatthewMckee4 on 2025-11-28 16:12_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-11-28 16:12_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-11-28 16:12_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-11-28 16:12_

---

_Renamed from "Fix override of final method summary" to "[ty] Fix override of final method summary" by @MatthewMckee4 on 2025-11-28 16:13_

---

_@MichaReiser approved on 2025-11-28 16:14_

Thank you

---

_Label `documentation` added by @MichaReiser on 2025-11-28 16:14_

---

_Label `ty` added by @MichaReiser on 2025-11-28 16:14_

---

_Comment by @AlexWaygood on 2025-11-28 16:14_

whooops, thank you!!

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 16:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-28 16:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 500 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @MichaReiser on 2025-11-28 16:18_

---

_Closed by @MichaReiser on 2025-11-28 16:18_

---

_Branch deleted on 2025-12-27 00:21_

---
