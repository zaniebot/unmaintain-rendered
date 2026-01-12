```yaml
number: 21434
title: "[ty] fix panic with direct recursive type alias"
type: pull_request
state: closed
author: mtshiba
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: direct-recursive-type-alias
created_at: 2025-11-13T19:18:40Z
updated_at: 2025-11-14T16:33:59Z
url: https://github.com/astral-sh/ruff/pull/21434
synced_at: 2026-01-12T15:57:23Z
```

# [ty] fix panic with direct recursive type alias

---

_@mtshiba_

## Summary

I found that trying to use a directly recursive type alias like this would result in a stack overflow:

```python
type Itself = Itself

def foo(Itself: Itself):
    x: Itself
```

This should be reported as an invalid type alias, but a stack overflow should be fixed immediately anyway. We now consider the value type to be `Unknown` in such cases.

## Test Plan

New test case in `mdtest\pep695_type_aliases.md`

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 19:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 19:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Marked ready for review by @mtshiba on 2025-11-13 19:31_

---

_Review requested from @carljm by @mtshiba on 2025-11-13 19:31_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-11-13 19:31_

---

_Review requested from @sharkdp by @mtshiba on 2025-11-13 19:31_

---

_Review requested from @dcreager by @mtshiba on 2025-11-13 19:31_

---

_Label `ty` added by @AlexWaygood on 2025-11-13 19:34_

---

_Label `bug` added by @AlexWaygood on 2025-11-13 19:34_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:249 on 2025-11-13 21:54_

Does this fix also work for compound types like list[Itself]?

---

_@MichaReiser reviewed on 2025-11-13 21:54_

---

_@carljm reviewed on 2025-11-13 23:18_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:249 on 2025-11-13 23:18_

Those generally already work, without this PR: https://play.ty.dev/a65c2d75-e9c1-4a74-85e3-f96db2dd9c60

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:252 on 2025-11-13 23:21_

IMO it's a bit odd to test that you can annotate a parameter as `Itself`, and then use that parameter again as an annotation. It may work due to how we treat `Unknown` in a type annotation, but I don't think this is something we want to commit to in a test.

I would probably just have done this instead:

```suggestion
def foo(x: Itself):
    reveal_type(x)  # Unknown
```

Is there a particular reason why you think it's important to test this "annotated value as annotation" behavior?

---

_@carljm approved on 2025-11-13 23:23_

Thank you!

---

_@carljm reviewed on 2025-11-13 23:25_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:249 on 2025-11-13 23:25_

Another case that may be worth testing is multi-step direct recursion:

```
type A = B
type B = A
```

---

_@mtshiba reviewed on 2025-11-14 09:08_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:252 on 2025-11-14 09:08_

In fact, in that example, no panic occurs, and the revealed type is `Never`.

https://play.ty.dev/4732e30a-301c-4753-b95f-aed34cf08585

Should the original code be placed in the corpus as a regression test?


---

_Converted to draft by @mtshiba on 2025-11-14 10:08_

---

_@mtshiba reviewed on 2025-11-14 10:10_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:249 on 2025-11-14 10:10_

Hmm, unfortunately, it seems the panic still persists in the case of mutually recursive aliases:

```python
type A = B
type B = A

def bar(a: B):
    x: a
```

Therefore, the measure is insufficient, and we should check the consequences of eagerly expanding a type alias value type before we first use of it.
This may be an issue that needs to be resolved in #20566.

---

_Comment by @mtshiba on 2025-11-14 13:49_

Detecting mutually recursive invalid type aliases was harder than expected, and I decided to address it in #20566.

Related commits:
https://github.com/astral-sh/ruff/pull/20566/commits/ac44afa7390034422bb521393095cc02394eff6d
https://github.com/astral-sh/ruff/pull/20566/commits/2a9b9a26db73c058994f3c58d8de324ae3366005

I will close this PR.

---

_Closed by @mtshiba on 2025-11-14 14:41_

---

_Branch deleted on 2025-11-14 14:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:252 on 2025-11-14 16:33_

Ah, I wondered if this part was necessary to reproduce the panic. That explains why I didn't catch this case in my previous work on recursive type aliases. Makes it much less likely people will run into this, but of course it should still be fixed.

I think it's OK for a test like this to be in mdtests, I would just expect some commentary (specifically about annotating with a value type) that "this is a very strange thing to do, but this is a regression test to ensure it doesn't panic"

---

_@carljm reviewed on 2025-11-14 16:33_

---
