```yaml
number: 21399
title: "[ty] Improve semantic token classification for names"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/semantic-tokens-improvements
created_at: 2025-11-12T09:50:30Z
updated_at: 2025-11-12T16:34:27Z
url: https://github.com/astral-sh/ruff/pull/21399
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Improve semantic token classification for names

---

_Pull request opened by @MichaReiser on 2025-11-12 09:50_

## Summary

This PR addresses the first TODO in https://github.com/astral-sh/ty/issues/791

> Improve classification for name tokens that are imported from other modules. Currently, these are classified based on their types, which often means they're classified as variables when they should be classes, etc.

The semantic token provider was written before we added the more involved `definitions_for_name` that resolves definitions across modules. This PR changes the semantic token provider to use `definitions_for_name` instead of rolling its own, very limited definition lookup (that only performed a lookup in the same scope).

This should fix many miss classifications, like that `float` was classified as a `Variable` instead of a `Class`. 

This PR further fixes some more misclasification:

* Classify `self` in `self.x` within a method as `SelfParamter`
* Set the `Definition` modifier for assignments: `x` in `x = 10` is a definition
* Classify legacy type aliases as `Class`es
* Classify names referencing a parameter as `Parameter`

## Test plan

Added tests, I compared all classification changes with what Pylance does




---

_Label `server` added by @MichaReiser on 2025-11-12 09:50_

---

_Label `ty` added by @MichaReiser on 2025-11-12 09:50_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 09:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-12 09:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-11-12 10:01_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:640 on 2025-11-12 10:01_

I realized right after landing the *go to definition for float and complex* PR that using `FileWithRange` here is a stupid idea. I switched to using `FileWithRange` to suppress the docstring (we always pick the first docstring which ended up to be `int` which is rather confusing). However, returning `FileWithRange` here breaks semantic highlighting because a `FileWithRange` has no definition.

That's why I opted in this PR to keep using `Definition` but return the definition for `float` first (or `complex`) so that we show the docstring of `float`. 

This is not 100% correct because `float` is a union in this position, but it also doesn't feel entirely wrong (and sort of what I'd expect as a user).

---

_Marked ready for review by @MichaReiser on 2025-11-12 10:04_

---

_Review requested from @carljm by @MichaReiser on 2025-11-12 10:04_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-12 10:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-12 10:04_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-12 10:04_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-12 10:10_

---

_@Gankra reviewed on 2025-11-12 14:54_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:640 on 2025-11-12 14:54_

Could you add a comment to the `.rev()` explaining that it makes `float` / `complex` come first in their own listings?

---

_@Gankra approved on 2025-11-12 14:55_

---

_Merged by @MichaReiser on 2025-11-12 16:34_

---

_Closed by @MichaReiser on 2025-11-12 16:34_

---

_Branch deleted on 2025-11-12 16:34_

---
