```yaml
number: 2480
title: "Missing type narrowing for `NoReturn` control flow"
type: issue
state: closed
author: mvaldi
labels: []
assignees: []
created_at: 2026-01-13T14:03:46Z
updated_at: 2026-01-13T14:05:05Z
url: https://github.com/astral-sh/ty/issues/2480
synced_at: 2026-01-13T14:31:56Z
```

# Missing type narrowing for `NoReturn` control flow

---

_@mvaldi_

# Type narrowing not working after calling a `NoReturn` function

Hi! I love this project. It's blazingly fast and i use it daily. i found what i believe is a bug with type narrowing after calling a function typed as `NoReturn`

## Summary

When a function with return type `NoReturn` is called inside an `if` block to guard against `None`, `ty` does not narrow the type of the variable in subsequent code. The variable still shows as `str | None` instead of being narrowed to `str`.

## Minimal Reproduction

```python
from typing import NoReturn


def raise_error() -> NoReturn:
    """Raise an error."""
    msg = "This is a test error"
    raise ValueError(msg)


data: dict[str, str] = {}

api_key = data.get("api_key")
if not api_key:
    raise_error()


def use_api_key(api_key: str) -> None:
    """Use an API key."""
    api_key.startswith("sk_")


use_api_key(api_key)  # <-- Error reported here
```

## Expected Behavior

After the `if not api_key: raise_error()` block, `ty` should understand that:
1. The `raise_error()` function **never returns** (it's typed as `NoReturn`)
2. Therefore, if execution continues past that point, `api_key` cannot be `None` or falsy
3. The type of `api_key` should be narrowed from `str | None` to `str`

This is the same behavior as if you used `raise ValueError()` directly in the `if` block.

## Actual Behavior

`ty` reports an error on the `use_api_key(api_key)` call:

```
Argument to function `use_api_key` is incorrect: Expected `str`, found `str | None`
  invalid-argument-type
```

## Environment

- **ty version**: 0.0.11
- **OS**: Linux
- **Python version**: 3.12+

## Workaround

Currently, I have to add an explicit `api_key = ""` after the guard, which defeats the purpose of having the `NoReturn` typed function.



### Version

0.0.11

---

_Comment by @AlexWaygood on 2026-01-13 14:05_

Thanks! this is #690

---

_Closed by @AlexWaygood on 2026-01-13 14:05_

---
