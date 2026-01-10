```yaml
number: 18503
title: "[ty] implement disjointness of Callable vs SpecialForm"
type: pull_request
state: merged
author: carljm
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: cjm/callable-vs-specialform
created_at: 2025-06-06T16:49:31Z
updated_at: 2025-06-11T10:51:58Z
url: https://github.com/astral-sh/ruff/pull/18503
synced_at: 2026-01-10T18:45:04Z
```

# [ty] implement disjointness of Callable vs SpecialForm

---

_Pull request opened by @carljm on 2025-06-06 16:49_

## Summary

Fixes https://github.com/astral-sh/ty/issues/557

## Test Plan

Stable property tests succeed with a million iterations. Added mdtests.


---

_Review requested from @AlexWaygood by @carljm on 2025-06-06 16:49_

---

_Review requested from @sharkdp by @carljm on 2025-06-06 16:49_

---

_Review requested from @dcreager by @carljm on 2025-06-06 16:49_

---

_Label `ty` added by @carljm on 2025-06-06 16:49_

---

_@AlexWaygood requested changes on 2025-06-06 16:52_

This special form is callable: https://github.com/astral-sh/ruff/blob/1274521f9fda19507921c85442d0ca991b67b846/crates/ty_python_semantic/src/types/special_form.rs#L102

---

_Comment by @github-actions[bot] on 2025-06-06 16:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Converted to draft by @carljm on 2025-06-06 16:53_

---

_Comment by @carljm on 2025-06-06 22:21_

@AlexWaygood It looks like `ChainMap`, `DefaultDict`, `Counter`, `Deque`, and `OrderedDict` (the collections proxies) are callable at runtime, but in typeshed are annotated as `_Alias`, which has no `__call__`. Do you think we should model their runtime callability as a special case, or trust typeshed here?

---

_Comment by @AlexWaygood on 2025-06-06 22:27_

The collections proxies are callable, but the builtins proxies aren't?! That's... Odd. I had in my head that all the legacy stdlib aliases were not callable!

We should probably treat them as callable if they are at runtime — can we just delegate to the constructor signatures of the classes they're aliasing? I don't love the idea of hardcoding their constructors' signatures somewhere in our model — some of those classes have reasonably complicated constructor methods IIRC

---

_Comment by @carljm on 2025-06-07 02:08_

> can we just delegate to the constructor signatures of the classes they're aliasing

Yeah, that should be possible. It's beyond the scope of what I was looking to do in this PR, but I added a TODO for it.

---

_Marked ready for review by @carljm on 2025-06-07 02:08_

---

_Comment by @carljm on 2025-06-07 04:55_

> can we just delegate to the constructor signatures of the classes they're aliasing

In that case, is there any reason not to just treat them as direct aliases? That is, remove their SpecialForm type entirely and act as if `typing.pyi` imported them directly from `collections`?

---

_Comment by @AlexWaygood on 2025-06-07 07:55_

[PEP 585](https://peps.python.org/pep-0585/#implementation) states:

> Importing [the legacy aliases] from `typing` is deprecated. Due to [PEP 563](https://peps.python.org/pep-0563/) and the intention to minimize the runtime impact of typing, this deprecation will not generate DeprecationWarnings. Instead, type checkers may warn about such deprecated usage when the target version of the checked program is signalled to be Python 3.9 or newer. It’s recommended to allow for those warnings to be silenced on a project-wide basis.

I think pyright implements this deprecation, and it would be nice if we could eventually as well. If we didn't distinguish between `typing.ChainMap` and `collections.ChainMap` at all in our model, that might be tricky?

Edit: Hmm, come to think of it, though, we'll have the same "tricky" issue when it comes to implementing deprecation warnings for `typing.Iterator`, `typing.Iterable`, `typing.Mapping`, etc. According to typeshed, these aren't just aliases for stdlib classes in `collections.abc` -- according to typeshed, these are actually _defined_ in `typing` (and re-exported in `collections.abc`). There have been numerous attempts to move the class definitions to the `collections.abc` stub, but it's tough because existing type checkers hardcode assumptions that these class definitions will be found in `typing`.

Given that we'll maybe have to solve that issue for the `collections.abc` aliases anyway, maybe it doesn't actually help us at all to represent `typing.ChainMap` as a different type internally to `collections.ChainMap`.

---

_@AlexWaygood approved on 2025-06-07 07:55_

---

_@AlexWaygood reviewed on 2025-06-07 08:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/special_form.rs`:255 on 2025-06-07 08:21_

```suggestion
            // TypedDict can be called as a constructor to create TypedDict types
```

---

_Comment by @AlexWaygood on 2025-06-07 10:20_

> The collections proxies are callable, but the builtins proxies aren't?! That's... Odd. I had in my head that all the legacy stdlib aliases were not callable!

It looks like the builtin aliases are specifically excluded from being callable at runtime by passing the `inst=False` keyword argument: https://github.com/python/cpython/blob/24069fbca861a5904ee7718469919e84828f22e7/Lib/typing.py#L2701-L2785. Why the decision was made to treat these aliases differently to other stdlib aliases, I'm not sure.

---

_Merged by @carljm on 2025-06-10 20:25_

---

_Closed by @carljm on 2025-06-10 20:25_

---

_Branch deleted on 2025-06-10 20:25_

---

_Label `bug` added by @dhruvmanila on 2025-06-11 10:51_

---
