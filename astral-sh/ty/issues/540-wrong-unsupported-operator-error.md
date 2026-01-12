```yaml
number: 540
title: Wrong  unsupported-operator error
type: issue
state: closed
author: pmareke
labels: []
assignees: []
created_at: 2025-05-28T19:29:42Z
updated_at: 2025-05-28T20:43:47Z
url: https://github.com/astral-sh/ty/issues/540
synced_at: 2026-01-12T15:54:23Z
```

# Wrong  unsupported-operator error

---

_@pmareke_

### Summary

Hi team, first of all thanks for ty, it's an amazing tool, I'm using a lot on my personal projects.

In one of them I have the following error:

```sh
error[unsupported-operator]: Operator `<` is not supported for types `float` and `None`, in comparing `float` with `int | float | None`
```

With the following code:

```python
@dataclass
class MyClass:
    min: float | None = None


def lower_value(my_class: MyClass, value: str) -> bool:
    if my_class.min:
        return float(value) < my_class.min
    return False
```

Same with this code:

```python
@dataclass
class MyClass:
    min: float | None = None


def lower_value(my_class: MyClass, value: str) -> bool:
    if my_class.min is not None:
        return float(value) < my_class.min
    return False
```

And I think the code is correct (this code in `mypy` doesn't raise any error).

Thanks and sorry if this is not a bug or it's repeated.

### Version

0.0.1a7

---

_Comment by @jelle-openai on 2025-05-28 19:32_

This is an instance of #164.

---

_Comment by @pmareke on 2025-05-28 19:33_

> This is an instance of [#164](https://github.com/astral-sh/ty/issues/164).

Thanks and sorry @jelle-openai.

I searched for "unsupported-operator" and I didn't see anything similar.

---

_Closed by @pmareke on 2025-05-28 19:33_

---

_Reopened by @carljm on 2025-05-28 20:43_

---

_Closed by @carljm on 2025-05-28 20:43_

---
