```yaml
number: 1258
title: "`isinstance` type narrowing should work even when the `isinstance` result is assigned to a variable"
type: issue
state: closed
author: benglewis
labels:
  - wish
  - narrowing
assignees: []
created_at: 2025-09-25T16:21:18Z
updated_at: 2025-11-18T16:10:39Z
url: https://github.com/astral-sh/ty/issues/1258
synced_at: 2026-01-10T01:58:59Z
```

# `isinstance` type narrowing should work even when the `isinstance` result is assigned to a variable

---

_Issue opened by @benglewis on 2025-09-25 16:21_

### Summary

Currently the following code does not work to type narrow, but I believe that it should:

```python
def fun(a: str | Path):
  a_is_path = isinstance(a, Path)
  if a_is_path:
     print(a.is_dir())
```

(Obviously this isn't a good use case to assign a variable, but imagine that there are multiple places where you do that check in multiple very different logic blocks, it would be nice to be able to assign to a variable instead)

### Version

ty 0.0.1-alpha.21 (ef52a19 2025-09-19)

---

_Label `wish` added by @AlexWaygood on 2025-09-25 16:30_

---

_Label `narrowing` added by @AlexWaygood on 2025-09-25 16:30_

---

_Comment by @erictraut on 2025-09-25 17:29_

This feature is typically called [aliased conditional expressions](https://microsoft.github.io/pyright/#/type-concepts-advanced?id=aliased-conditional-expression). I think pyright is currently the only major Python type checker that implements it. I borrowed the concept and the terminology from the TypeScript compiler. It's a tricky feature to implement correctly because the type checker needs to prove that the alias variable is not reassigned along any code path and that none of the subexpressions used in the conditional expressions have potentially been changed along any code path.

---

_Comment by @ericludvigs on 2025-10-06 11:24_

Seconding wish for this feature, ran into false positive error while adapting [this code](https://github.com/sagasv5-xw/map2map/blob/67a56215cb4c252c769dd84dc0fa5e0abce1c648/map2map/data/fields.py#L93C1-L96C54).

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Closed by @carljm on 2025-11-15 02:00_

---
