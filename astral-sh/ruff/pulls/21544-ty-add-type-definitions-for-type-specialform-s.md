```yaml
number: 21544
title: "[ty] Add type definitions for `Type::SpecialForm`s"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: alex/gototype-specialform
created_at: 2025-11-20T17:48:03Z
updated_at: 2025-11-20T18:14:31Z
url: https://github.com/astral-sh/ruff/pull/21544
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add type definitions for `Type::SpecialForm`s

---

_Pull request opened by @AlexWaygood on 2025-11-20 17:48_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1601, enabling you to click on `Literal` in an inlay hint and jump to the definition of `typing.Literal` in typeshed

## Test Plan

New snapshots!


---

_Review requested from @Gankra by @AlexWaygood on 2025-11-20 17:48_

---

_Label `server` added by @AlexWaygood on 2025-11-20 17:48_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-20 17:48_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-20 17:48_

---

_Label `ty` added by @AlexWaygood on 2025-11-20 17:48_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-20 17:48_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-20 17:48_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 17:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-20 17:51_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/special_form.rs`:549 on 2025-11-20 17:55_

What cases is the rsplit handling?

---

_@Gankra approved on 2025-11-20 17:56_

Rad!

---

_@AlexWaygood reviewed on 2025-11-20 17:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/special_form.rs`:549 on 2025-11-20 17:59_

hmm, I started typing out a long explanation for the hack but then realised I can make it less hacky -- thanks for making me look at it again! 😄

---

_@AlexWaygood reviewed on 2025-11-20 18:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/special_form.rs`:549 on 2025-11-20 18:09_

hack is gone in https://github.com/astral-sh/ruff/pull/21544/commits/9c3aabb5d03b66172d915c5024c3cfb03c0755e3

---

_@AlexWaygood reviewed on 2025-11-20 18:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:1114 on 2025-11-20 18:10_

not sure how we missed this 😆

---

_Merged by @AlexWaygood on 2025-11-20 18:14_

---

_Closed by @AlexWaygood on 2025-11-20 18:14_

---

_Branch deleted on 2025-11-20 18:14_

---
