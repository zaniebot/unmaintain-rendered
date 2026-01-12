```yaml
number: 20084
title: "[ty] Add support for PEP 800"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/disjoint-base
created_at: 2025-08-25T17:36:39Z
updated_at: 2025-08-25T18:39:07Z
url: https://github.com/astral-sh/ruff/pull/20084
synced_at: 2026-01-12T15:56:54Z
```

# [ty] Add support for PEP 800

---

_@AlexWaygood_

## Summary

This PR adds support for [PEP 800](https://peps.python.org/pep-0800/). Since ty's implementation of disjointness was the original inspiration for the PEP, nearly everything was already implemented; this PR mainly consists of renaming internal concepts to match up with the terms used by the PEP, and hooking things up so that we recognize the effect the new `@disjoint_base` decorator has.

## Test Plan

Mdtests and snapshots


---

_Label `ty` added by @AlexWaygood on 2025-08-25 17:36_

---

_Comment by @github-actions[bot] on 2025-08-25 17:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-25 17:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~76MB
+     memo metadata = ~80MB

```
</details>


---

_Comment by @AlexWaygood on 2025-08-25 17:57_

> No ecosystem changes detected ✅

A bit of a bummer -- I thought this would improve our understanding of disjointness a fair bit now that typeshed [has added `@disjoint_base`](https://github.com/python/typeshed/pull/14599) across all applicable stdlib classes. I expected to see at least some ecosystem hits.

> Memory usage changes were detected when running on open source projects

Interestingly exactly the same increase is being reported over in #20085 -- not sure if this job is getting flaky again...?

---

_@AlexWaygood reviewed on 2025-08-25 17:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/instance_layout_conf…_-_Tests_for_ty's_`inst…_-_Built-ins_with_impli…_(f5857d64ce69ca1d).snap`:199 on 2025-08-25 17:59_

of course, unlike the classes that we hardcode as being disjoint bases, we don't _know_ that two arbitrary classes decorated as being disjoint bases are marked as such due to the way they're implemented in a C extension. You could add that decorator to a class for whatever reason you like.

It holds true for all of typeshed's disjoint bases, though, and it's _probably_ a good guess in general...?

---

_Marked ready for review by @AlexWaygood on 2025-08-25 18:00_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-25 18:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-25 18:00_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-25 18:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-25 18:00_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:3660 on 2025-08-25 18:02_

In principle we could eliminate this method and always look at the typeshed fallback class (and its decorator), right? This is just a small performance optimization (maybe) at the cost of preserving some special-casing that we technically shouldn't need anymore?

---

_@carljm approved on 2025-08-25 18:05_

Heh, I wondered after the typeshed PR if you were just gonna go ahead and do this. Looks good!

---

_Comment by @AlexWaygood on 2025-08-25 18:06_

> Heh, I wondered after the typeshed PR if you were just gonna go ahead and do this

Ah, I'm an open book I see!

---

_@AlexWaygood reviewed on 2025-08-25 18:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3660 on 2025-08-25 18:08_

Exactly. I'll try getting rid of it and see what Codspeed says!

---

_@AlexWaygood reviewed on 2025-08-25 18:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3660 on 2025-08-25 18:38_

heh, Codspeed says that removing the method is no regression at all! Which means that, excluding tests, this PR now removes lines of code on net 🚀

---

_Merged by @AlexWaygood on 2025-08-25 18:39_

---

_Closed by @AlexWaygood on 2025-08-25 18:39_

---

_Branch deleted on 2025-08-25 18:39_

---
