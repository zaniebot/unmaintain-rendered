```yaml
number: 21388
title: "[ty] Fix goto for `float` and `complex` in type annotation positions"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/float-complex-goto
created_at: 2025-11-11T18:23:36Z
updated_at: 2025-11-12T07:54:26Z
url: https://github.com/astral-sh/ruff/pull/21388
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Fix goto for `float` and `complex` in type annotation positions

---

_Pull request opened by @MichaReiser on 2025-11-11 18:23_

## Summary

I looked into fixing https://github.com/astral-sh/ty/issues/1470, specifically that `float` and `complex` are highlighted differently (as Variables instead of classes) compared to `int`, `bool`. 

The reason for the difference in highlighting is that ty (following the typing spec) expands `float` and `complex` in type annotation positions to `float | int` and `complex | float | int`, a union. 

However, this difference wasn't obvious to me as a user because clicking on` float` or `complex` jumped right to `class float` and `class complex` in `builtins.pyi` rather than showing all definitions. 

This PR fixes that which, IMO, makes the experience more consistent:

* Hovering `float` in type annotation positions shows `float | int`
* We show `float | int` within type annotations over just `float` or `int`

I think we might still want to fix issue https://github.com/astral-sh/ty/issues/1470 separately by highlighting `float` and `complex` the same as we would any other type alias.

## Test

I added a test for goto definition. I also added a test for semantic highlighting just so that it's already in place.

---

_Label `server` added by @MichaReiser on 2025-11-11 18:23_

---

_Label `ty` added by @MichaReiser on 2025-11-11 18:23_

---

_Label `server` added by @MichaReiser on 2025-11-11 18:23_

---

_Label `ty` added by @MichaReiser on 2025-11-11 18:23_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 18:25_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 18:27_


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

_@MichaReiser reviewed on 2025-11-11 18:32_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:633 on 2025-11-11 18:32_

This feels very silly but I'm not sure if there's a better way of doing this.

---

_Marked ready for review by @MichaReiser on 2025-11-11 18:32_

---

_Review requested from @carljm by @MichaReiser on 2025-11-11 18:32_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-11 18:32_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-11 18:32_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-11 18:32_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:632 on 2025-11-11 19:13_

could && chain these together?

---

_@Gankra approved on 2025-11-11 19:18_

I agree this is kinda silly but I guess it works fine? Could you add a hover test too? I feel like hover and goto might want to diverge here, so it would be nice to track them both.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-11 19:24_

---

_Comment by @MichaReiser on 2025-11-11 19:28_

> I feel like hover and goto might want to diverge here, so it would be nice to track them both.

What's the reason why you think that hover should diverge? Is it because we want to show the documentation for `float` (I'm pretty sure this breaks with this change)

---

_Review comment by @carljm on `crates/ty_ide/src/semantic_tokens.rs`:1205 on 2025-11-11 19:40_

Do we want to mark this (and the other cases of `float: Variable`) in the test as ideally TODOs?

---

_Review comment by @carljm on `crates/ty_ide/src/semantic_tokens.rs`:1205 on 2025-11-11 19:41_

It seems to me like it would be reasonable (and not hard) to unconditionally mark any union of ClassLiteral types as `Class` for purposes of semantic tokens?

---

_Comment by @Gankra on 2025-11-11 19:43_

I'm kinda 🤷‍♀️ on literally any behaviour here, I just want it written down somewhere

---

_@carljm approved on 2025-11-11 19:45_

---

_@MichaReiser reviewed on 2025-11-12 07:27_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:1205 on 2025-11-12 07:27_

While this works, it's just a work around for fixing how semantic tokens looks up symbols by name. I rather invest time in the proper fix.

---

_Merged by @MichaReiser on 2025-11-12 07:54_

---

_Closed by @MichaReiser on 2025-11-12 07:54_

---

_Branch deleted on 2025-11-12 07:54_

---
