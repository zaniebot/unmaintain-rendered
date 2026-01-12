```yaml
number: 2179
title: Support type narrowing for union of tuples based on element types.
type: issue
state: closed
author: tkukushkin
labels:
  - narrowing
assignees: []
created_at: 2025-12-23T06:01:27Z
updated_at: 2025-12-24T01:15:52Z
url: https://github.com/astral-sh/ty/issues/2179
synced_at: 2026-01-12T15:54:26Z
```

# Support type narrowing for union of tuples based on element types.

---

_@tkukushkin_

### Summary

Type narrowing should work for union of tuples:

```python
def foo(x: tuple[int, int] | tuple[None, None]) -> None:
    if x[0] is not None:
        reveal_type(x)  # should be tuple[int, int]
        reveal_type(x[1])    # should be int
```

Mypy and pyright support it, ty doesn't.

[Ty Playground](https://play.ty.dev/ffb81094-232a-4f46-b644-35258c99876a), [Mypy Playground](https://mypy-play.net/?mypy=latest&python=3.12&gist=2cfdbd4496f102d23daa2ec17ab39480), [Pyright playground](https://pyright-play.net/?strict=true&code=CYUwZgBGD20BQA8BcEAuBXADgGxAbQEsA7VAGgmNQF0IAfNLXPAOWiJHNfaoEoIBaAHwQuIJACgIUipAR4ADDQIBnCEWioRbMZOl6ATiABuIAIbYA%2BqgCemEIh669UwyfNXb9uQEZeQA).


### Version

0.0.5

---

_Label `narrowing` added by @AlexWaygood on 2025-12-23 09:29_

---

_Added to milestone `Stable` by @carljm on 2025-12-23 20:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-23 22:53_

---

_Closed by @charliermarsh on 2025-12-24 01:15_

---
