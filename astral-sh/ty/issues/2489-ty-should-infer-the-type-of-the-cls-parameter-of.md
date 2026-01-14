```yaml
number: 2489
title: "ty should infer the type of the `cls` parameter of `__new__`"
type: issue
state: open
author: arjenzorgdoc
labels:
  - bug
assignees: []
created_at: 2026-01-14T10:33:49Z
updated_at: 2026-01-14T10:37:06Z
url: https://github.com/astral-sh/ty/issues/2489
synced_at: 2026-01-14T11:33:10Z
```

# ty should infer the type of the `cls` parameter of `__new__`

---

_@arjenzorgdoc_

The `cls` parameter of `__new__` should be inferred as `type[Self]`. ty says it's `Unknown`

```python
from typing import reveal_type

class C:
    def __new__(cls):
        reveal_type(cls)
        return super().__new__(cls)
```

- [pyright](https://pyright-play.net/?pythonVersion=3.14&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFCMDGFZAzm1AMIBc9UAqABNCwKFSopCAdwkAKFmwCUfQWoIly1WoQUVl-dQKIwAriBRQ2puiDlKAdBKmyqeg0A): `Type of "cls" is "type[Self@C]"`  
- [ty](https://play.ty.dev/e5971a97-8ed2-45f4-8a08-98b6105a3754): `Revealed type: Unknown (revealed-type) [Ln 6, Col 21]`
- [mypy](https://mypy-play.net/?mypy=latest&python=3.14&gist=9fc1160fed504745c96441caa2465334&flags=check-untyped-defs): `main.py:5: note: Revealed type is "type[__main__.C]"`

<details>
<summary>My actual use case is code like this:</summary>

```python
from typing import Self


class C:
    def __new__(cls: type[Self], x: object) -> Self:
        if type(x) is cls:
            return x
        return super().__new__(cls)
```
</details>

---

_Label `bug` added by @AlexWaygood on 2026-01-14 10:36_

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-14 10:36_

---

_Comment by @AlexWaygood on 2026-01-14 10:37_

Thanks! Hmm, I thought we'd fixed this a while back :(

---
