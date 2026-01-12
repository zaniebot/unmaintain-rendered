```yaml
number: 13121
title: Type hinting Numpy arrays raises UP037 warning
type: issue
state: open
author: MattTheCuber
labels:
  - bug
assignees: []
created_at: 2024-08-27T11:50:31Z
updated_at: 2025-07-09T23:54:24Z
url: https://github.com/astral-sh/ruff/issues/13121
synced_at: 2026-01-12T15:54:52Z
```

# Type hinting Numpy arrays raises UP037 warning

---

_@MattTheCuber_

Type hinting Numpy arrays raises the warning "Remove quotes from type annotation Ruff (UP037)". From our research, this seems to be the only way of type hinting that satisfies Pyright.

```py
from nptyping import Int, NDArray, Shape

# Warning UP037 raised on "2, 3" below
def test(data: NDArray[Shape["2, 3"], Int]):
    pass
```

---

_Label `bug` added by @charliermarsh on 2024-08-27 14:03_

---

_Comment by @nvandamme on 2024-12-06 14:32_

Confirming on my side with multiple warnings:
```python
import nptyping as npt
points: (
        npt.NDArray[npt.Shape["*, [x, y, z]"], npt.Number]  # noqa: F722
        | npt.NDArray[npt.Shape["[x, y, z]"], npt.Number]  # noqa: F821
        | npt.NDArray[npt.Shape["3, *"], npt.Number]  # noqa: F722
        | Sequence[npt.Number]
        | Sequence[Sequence[npt.Number]]
    )
```

---

_Comment by @MeGaGiGaGon on 2025-07-09 23:54_

This looks to have been covered in/might be a duplicate of #6014

Also from that issue, https://github.com/astral-sh/ruff/issues/6014#issuecomment-1650235989 `nptyping.Shape` is just an alias for `typing.Literal` [as said in the `nptyping` docs](https://github.com/ramonhagenaars/nptyping/blob/master/USERDOCS.md#shape-expressions), so you can use it instead to make `UP037` happy https://play.ruff.rs/368f82f7-a4fe-442e-a18d-f7ce61bc927f

---
