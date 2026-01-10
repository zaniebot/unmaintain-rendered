---
number: 2430
title: "A property `str` shadowing str results in an invalid-type-form error"
type: issue
state: closed
author: rchui
labels: []
assignees: []
created_at: 2026-01-09T23:16:21Z
updated_at: 2026-01-10T00:01:36Z
url: https://github.com/astral-sh/ty/issues/2430
synced_at: 2026-01-10T01:48:23Z
---

# A property `str` shadowing str results in an invalid-type-form error

---

_Issue opened by @rchui on 2026-01-09 23:16_

### Summary

The issue is that ty resolves str in the type annotation to the .str property defined in the same class, rather than the builtin str type.

https://play.ty.dev/b20b7f2e-34de-442a-8867-30a786c58dba

```python
# shadow_example.py
class Foo:
    @property
    def str(self) -> "StringAccessor":
        """This property shadows the builtin str type."""
        return StringAccessor()

    def greet(self, name: str) -> str:
        """ty thinks 'str' here refers to the property above, not builtin str."""
        return f"Hello, {name}"


class StringAccessor:
    pass
```

Run ty check shadow_example.py:

```python
error[invalid-type-form]: Variable of type `property` is not allowed in a type expression
 --> shadow_example.py:8:24
  |
6 |         return StringAccessor()
7 |
8 |     def greet(self, name: str) -> str:
  |                          ^^^
9 |         """ty thinks 'str' here refers to the property above, not builtin str."""
  |
```

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Comment by @MeGaGiGaGon on 2026-01-09 23:26_

Thank you for the bug report. For a more thorough explanation of why this happens see [this comment](https://github.com/astral-sh/ty/issues/900#issuecomment-3151616356), but this is expected behavior. ty respects the normal runtime name resolution rules, and at the definition of `greet` `str` is that property. You can see this if you try to use `str` to make a default value for that code:
https://play.ty.dev/91a3122d-ad78-431d-8dea-321920d99334
```py
# shadow_example.py
class Foo:
    @property
    def str(self) -> "StringAccessor":
        """This property shadows the builtin str type."""
        return StringAccessor()

    def greet(self, name: str = str()) -> str:
        """ty thinks 'str' here refers to the property above, not builtin str."""
        return f"Hello, {name}"


class StringAccessor:
    pass
```
At runtime this crashes with `TypeError: 'property' object is not callable` at the `str()`, showing that ty's interpretation is correct.

---

_Comment by @carljm on 2026-01-10 00:01_

Recommend aliasing `str` (e.g. `builtin_str = str`) at top level of your module, and then annotating using `builtin_str`, to get around this, if you must have a method/property named `str`.

---

_Closed by @carljm on 2026-01-10 00:01_

---
