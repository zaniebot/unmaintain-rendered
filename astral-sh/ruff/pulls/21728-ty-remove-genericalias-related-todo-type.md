```yaml
number: 21728
title: "[ty] Remove `GenericAlias`-related todo type"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/remove-genericalias-todo
created_at: 2025-12-01T12:18:32Z
updated_at: 2025-12-01T13:02:40Z
url: https://github.com/astral-sh/ruff/pull/21728
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Remove `GenericAlias`-related todo type

---

_Pull request opened by @sharkdp on 2025-12-01 12:18_

## Summary

If you manage to create an `typing.GenericAlias` instance without us knowing how that was created, then we don't know what to do with this in a type annotation. So it's better to be explicit and show an error instead of failing silently with a `@Todo` type.

## Test Plan

* New Markdown tests
* Zero ecosystem impact


---

_Label `ty` added by @sharkdp on 2025-12-01 12:18_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:22_


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

_Renamed from "[ty] Remove GenericAlias todo type" to "[ty] Remove `GenericAlias`-related todo type" by @sharkdp on 2025-12-01 12:36_

---

_Marked ready for review by @sharkdp on 2025-12-01 12:36_

---

_Review requested from @carljm by @sharkdp on 2025-12-01 12:36_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-01 12:36_

---

_Review requested from @dcreager by @sharkdp on 2025-12-01 12:36_

---

_@sharkdp reviewed on 2025-12-01 12:37_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/generic_alias.md`:28 on 2025-12-01 12:37_

No other type checker supports this. They don't emit an error, but their revealed type for `strings` (`GenericAlias`) seems… strange.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:15 on 2025-12-01 12:38_

I'm inclined to also get rid of the TODO a couple of lines above this. I don't think it's something we actually want to do 😆

---

_@AlexWaygood approved on 2025-12-01 12:38_

---

_@sharkdp reviewed on 2025-12-01 12:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:15 on 2025-12-01 12:42_

I don't understand this test at all, especially why it's so prominently at the top of this file :smile: 

---

_@AlexWaygood reviewed on 2025-12-01 12:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:15 on 2025-12-01 12:49_

yeah, maybe just delete the `GenericAlias` bits from this test? The test was originally added in https://github.com/astral-sh/ruff/commit/63e78b41cdcb6e5469998fe4fba97260b3a3e95c (as an incidental change that was part of a bigger PR, long before we supported `NewType` properly), and the TODO was added in https://github.com/astral-sh/ruff/commit/fab7d820bd6c8ea01f4fbd5ad8f92f3d333e5506 (also an incidental change that was part of a bigger PR)

---

_Merged by @sharkdp on 2025-12-01 13:02_

---

_Closed by @sharkdp on 2025-12-01 13:02_

---

_Branch deleted on 2025-12-01 13:02_

---
