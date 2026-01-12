```yaml
number: 916
title: Incorrect narrowing of class/global variables in nested scopes
type: issue
state: open
author: mtshiba
labels:
  - narrowing
assignees: []
created_at: 2025-07-30T16:16:16Z
updated_at: 2025-07-30T17:18:40Z
url: https://github.com/astral-sh/ty/issues/916
synced_at: 2026-01-12T15:54:24Z
```

# Incorrect narrowing of class/global variables in nested scopes

---

_@mtshiba_

### Summary

In the following code, `g` in the class scope `D` should be inferred as `str | None`, but is instead inferred as `str`.

```python
g: str | None = None

def f(flag: bool):
    class C:
        if flag:
            g = 1

        if g is not None:  # this `g` may refer to a class variable, or a global variable
            class D:
                # refers to the global `g`
                reveal_type(g)  # should be: str | None
```

Here, it appears that we have mistakenly applied the narrowing constraint `g is not None` to the global variable `g`.
However, simply checking the symbol table and not recording an eager snapshot if the class variable `C.g` is bound is not sufficient. In the following example, we can safely narrow the global `g` because we can assume that `C.g` is undefined in class `D`.

```python
g: str | None = None

def f(flag: bool):
    class C:
        if flag:
            g = None

        if g is not None:  # if this evaluates to `True`, `g` must refer to a global variable
            class D:
                reveal_type(g)  # revealed: str
```

This is already checked in [`mdtest/narrow/conditionals/nested.md`](https://github.com/astral-sh/ruff/blob/38049aae12896a0ca79ce3045b54ee1efd8103b7/crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md?plain=1#L420).

### Version

_No response_

---

_Comment by @mtshiba on 2025-07-30 16:43_

This issue, like https://github.com/astral-sh/ty/issues/875, is caused by incorrect modeling of odd Python semantics, but since this is a narrowing issue, I think the way to fix is a little different.

---

_Label `narrowing` added by @sharkdp on 2025-07-30 17:18_

---
