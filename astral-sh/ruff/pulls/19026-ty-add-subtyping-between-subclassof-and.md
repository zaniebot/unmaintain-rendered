```yaml
number: 19026
title: "[ty] Add subtyping between SubclassOf and CallableType"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: subclass-of-callable-has-relation-to
created_at: 2025-06-29T14:50:14Z
updated_at: 2025-07-16T21:12:30Z
url: https://github.com/astral-sh/ruff/pull/19026
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Add subtyping between SubclassOf and CallableType

---

_Pull request opened by @MatthewMckee4 on 2025-06-29 14:50_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Part of https://github.com/astral-sh/ty/issues/129

There were previously some false positives here.

## Test Plan

Updated `is_subtype_of.md` and `is_assignable_to.md`


---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1594 on 2025-06-29 14:52_

If it is not `None` then i think it is handled below in the `(Type::SubclassOf(subclass_of_ty), _)` arm.

---

_@MatthewMckee4 reviewed on 2025-06-29 14:52_

---

_Comment by @github-actions[bot] on 2025-06-29 14:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~49MB
+ TOTAL MEMORY USAGE: ~45MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB

nox (https://github.com/wntrblm/nox)
-     memo fields = ~66MB
+     memo fields = ~72MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo fields = ~97MB
+     memo fields = ~106MB

bandersnatch (https://github.com/pypa/bandersnatch)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

prefect (https://github.com/PrefectHQ/prefect)
+ error[invalid-argument-type] src/prefect/tasks.py:779:13: Argument to bound method `__init__` is incorrect: Expected `int | float | list[int | float] | ((int, /) -> list[int | float]) | None`, found `int | float | list[int | float] | (((int, /) -> list[int | float]) & ~<class 'NotSet'>) | (type[NotSet] & ~<class 'NotSet'>) | Unknown`
- Found 4186 diagnostics
+ Found 4187 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~717MB
+ TOTAL MEMORY USAGE: ~652MB

```
</details>


---

_Marked ready for review by @MatthewMckee4 on 2025-06-29 15:02_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-06-29 15:02_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-06-29 15:02_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-06-29 15:02_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-06-29 15:02_

---

_Comment by @MatthewMckee4 on 2025-06-29 15:05_

I think the mypy primer is correct.

Since `NotSet` is an empty class and therefore not a subclass of `float | int | list[float] | Callable[[int], list[float]])`.

Maybe a bit of an unfortunate diagnostic but it seems sound.

Their mistake is not marking the class with `@final` right?

---

_Label `ty` added by @ntBre on 2025-06-29 15:44_

---

_Renamed from "Add subtyping between SubclassOf and CallableType" to "[ty] Add subtyping between SubclassOf and CallableType" by @MatthewMckee4 on 2025-06-29 16:04_

---

_@carljm approved on 2025-07-03 02:07_

This looks good to me, and I ran the property tests at a million iterations with no failures.

---

_Comment by @carljm on 2025-07-03 02:13_

Oh, and I agree with your primer evaluation 100%! `x is not NotSet` should not narrow `type[NotSet]` out of a union unless `NotSet` is a final class. And we do handle it correctly if it's final: https://play.ty.dev/75aa3432-c763-4cb2-be00-e2297b386748

---

_Comment by @carljm on 2025-07-03 02:22_

The reason this accurate diagnostic was previously hidden is because `type` in typeshed has a very forgiving gradual `__call__` definition: `def __call__(self, *args: Any, **kwds: Any) -> Any: ...`. Our fallback here is just to see if `type` is assignable, and that gradual `__call__` definition makes an arbitrary instance of `type` assignable to any callable.


---

_Merged by @carljm on 2025-07-03 02:22_

---

_Closed by @carljm on 2025-07-03 02:22_

---

_Branch deleted on 2025-07-16 21:12_

---
