```yaml
number: 21541
title: "[ty] Add more TypeDetails to the display code"
type: pull_request
state: merged
author: Gankra
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/inlay-followup
created_at: 2025-11-20T16:24:32Z
updated_at: 2025-11-20T18:00:23Z
url: https://github.com/astral-sh/ruff/pull/21541
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add more TypeDetails to the display code

---

_Pull request opened by @Gankra on 2025-11-20 16:24_

As far as I know this change is largely non-functional, largely because of https://github.com/astral-sh/ty/issues/1601

It's possible some of these like `Type::KnownInstance` produce something useful sometimes. `LiteralString` is a new introduction, although its goto-type jumps to `str` which is a bit sad (considering that part of the SpecialForm discourse for now).

Also wrt the generics testing followup: turns out the snapshot tests were full of those already.

---

_Review requested from @carljm by @Gankra on 2025-11-20 16:24_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-20 16:24_

---

_Review requested from @sharkdp by @Gankra on 2025-11-20 16:24_

---

_Review requested from @dcreager by @Gankra on 2025-11-20 16:24_

---

_Label `server` added by @Gankra on 2025-11-20 16:24_

---

_Label `ty` added by @Gankra on 2025-11-20 16:24_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-20 16:25_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 16:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-20 16:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `internal` added by @Gankra on 2025-11-20 16:34_

---

_@AlexWaygood approved on 2025-11-20 17:04_

thank you!

---

_Merged by @Gankra on 2025-11-20 17:09_

---

_Closed by @Gankra on 2025-11-20 17:09_

---

_Branch deleted on 2025-11-20 17:09_

---

_Comment by @AlexWaygood on 2025-11-20 17:20_

> Also wrt the generics testing followup: turns out the snapshot tests were full of those already.

Hmm, really? I was asking specifically about generic-alias and generic subclass-of types in https://github.com/astral-sh/ruff/pull/21533#discussion_r2546190060. Those are both different variants to generic nominal-instance types, and I couldn't immediately see any snapshots for them

---

_Comment by @Gankra on 2025-11-20 18:00_

Ah I see, yes those are probably missing.

---
