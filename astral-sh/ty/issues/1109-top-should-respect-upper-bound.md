```yaml
number: 1109
title: "`Top[]` should respect upper bound"
type: issue
state: open
author: InSyncWithFoo
labels:
  - bug
assignees: []
created_at: 2025-08-31T16:56:16Z
updated_at: 2025-09-02T15:34:42Z
url: https://github.com/astral-sh/ty/issues/1109
synced_at: 2026-01-10T02:06:24Z
```

# `Top[]` should respect upper bound

---

_Issue opened by @InSyncWithFoo on 2025-08-31 16:56_

Minimal reproducible example ([playground](https://play.ty.dev/64e41ddb-8baf-43e0-abfd-64c45f0004c5)):

```python
class Covariant[T: int]:
    def _(self) -> T: ...

def _(covariant: Top[Covariant[Any]]):
    reveal_type(covariant)  # `Covariant[object]`, but should instead be `Covariant[int]`
```


---

_Comment by @JelleZijlstra on 2025-08-31 21:56_

Related: https://github.com/astral-sh/ruff/pull/19946 . The approach in that PR might also solve this issue.

There's a harder variant of this when a TypeVar has constraints:

```python
class Covariant[T: (int, str)]:
    def method(self) -> T: ...

def _(covariant: Top[Covariant[Any]]):
    reveal_type(covariant)  # ???
```
On Discord I suggested inferring this to be `Covariant[int | str]`, but it now occurs to me that we could instead infer `Covariant[int] | Covariant[str]`: that is, simply union together all the possible materializations.

---

_Comment by @InSyncWithFoo on 2025-08-31 22:29_

I agree; `Covariant[int] | Covariant[str]` seems much better.

---

_Label `bug` added by @dhruvmanila on 2025-09-01 08:06_

---

_Comment by @JelleZijlstra on 2025-09-02 14:00_

But the approach I suggest there could have pathological performance issues if you have a class that is generic over a lot of TypeVars with constraints. For example, materializing the class from https://github.com/python/mypy/issues/19622 (20 TypeVars with 2 constraints each) would generate a union of 2**20 elements.

So maybe we should just not simplify `Top[]` in this case, as we do for invariant TypeVars.

---

_Comment by @carljm on 2025-09-02 15:34_

> So maybe we should just not simplify `Top[]` in this case, as we do for invariant TypeVars.

This seems like a fine option; it just means we have to handle this additional case when implementing type relations for these unsimplified materializations.

---
