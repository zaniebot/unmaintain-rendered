```yaml
number: 21656
title: "[ty] Make inlay hint clickable in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/inlay-hint-clickable-playground
created_at: 2025-11-27T09:28:10Z
updated_at: 2025-11-27T12:29:12Z
url: https://github.com/astral-sh/ruff/pull/21656
synced_at: 2026-01-12T15:57:30Z
```

# [ty] Make inlay hint clickable in playground

---

_@MichaReiser_

## Summary

https://github.com/user-attachments/assets/c4f964ab-1a0b-4820-aa13-a1ba702f4252


---

_Label `playground` added by @MichaReiser on 2025-11-27 09:28_

---

_Label `ty` added by @MichaReiser on 2025-11-27 09:28_

---

_Label `playground` added by @MichaReiser on 2025-11-27 09:28_

---

_Label `ty` added by @MichaReiser on 2025-11-27 09:28_

---

_Review requested from @carljm by @MichaReiser on 2025-11-27 09:28_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-27 09:28_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-27 09:28_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 09:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 09:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 505 diagnostics
+ Found 507 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-11-27 09:59_

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:674 on 2025-11-27 09:59_

```suggestion
```

---

_@AlexWaygood approved on 2025-11-27 11:30_

love it

---

_Merged by @MichaReiser on 2025-11-27 12:29_

---

_Closed by @MichaReiser on 2025-11-27 12:29_

---

_Branch deleted on 2025-11-27 12:29_

---
