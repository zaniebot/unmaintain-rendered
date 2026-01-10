```yaml
number: 14169
title: "[red-knot] support * imports"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-11-07T16:24:58Z
updated_at: 2025-04-15T15:50:41Z
url: https://github.com/astral-sh/ruff/issues/14169
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] support * imports

---

_Issue opened by @carljm on 2024-11-07 16:24_

PRs:
- https://github.com/astral-sh/ruff/pull/16873
- https://github.com/astral-sh/ruff/pull/16899
- https://github.com/astral-sh/ruff/pull/16955
- https://github.com/astral-sh/ruff/pull/16923
- https://github.com/astral-sh/ruff/pull/16958
- https://github.com/astral-sh/ruff/pull/16959
- https://github.com/astral-sh/ruff/pull/17270
- astral-sh/ruff#17315
- astral-sh/ruff#17286
- astral-sh/ruff#17317
- https://github.com/astral-sh/ruff/pull/17373

---

_Label `red-knot` added by @carljm on 2024-11-07 16:24_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 16:24_

---

_Assigned to @AlexWaygood by @carljm on 2024-11-07 16:24_

---

_Removed from milestone `Red Knot 2024` by @carljm on 2025-01-09 17:43_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 17:43_

---

_Removed from milestone `Red Knot Q1 2025` by @carljm on 2025-03-27 18:32_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:32_

---

_Comment by @carljm on 2025-04-04 20:51_

@AlexWaygood I think whenever you move on from this, it would be good to close this issue and open a new specific issue for `__all__` support (or maybe just repurpose https://github.com/astral-sh/ty/issues/199 to cover all forms of `__all__` support?). Probably you were planning to do that anyway and this comment is redundant :)

---

_Comment by @AlexWaygood on 2025-04-04 20:52_

> Probably you were planning to do that anyway and this comment is redundant :)

Indeed ;)

---

_Comment by @sharkdp on 2025-04-06 15:43_

One thing that's potentially worth mentioning here is that we do not understand relatively basic imports from `numpy`:
```bash
▶ cat pyproject.toml   
[project]
name = "test-numpy"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "numpy>=2.2.4",
]

▶ cat main.py 
from numpy import array

▶ uv run red_knot check
error: lint:unresolved-import
 --> /tmp/test_numpy/main.py:1:19
  |
1 | from numpy import array
  |                   ^^^^^ Module `numpy` has no member `array`
  |

Found 1 diagnostic

```

---

_Comment by @AlexWaygood on 2025-04-06 15:53_

> ▶ uv run red_knot check
> error: lint:unresolved-import
>  --> /tmp/test_numpy/main.py:1:19
>   |
> 1 | from numpy import array
>   |                   ^^^^^ Module `numpy` has no member `array`
>   |

Hmm, I don't think that's an issue related to `*` imports? `numpy/__init__.py` has a stub file at [`numpy/__init__.pyi`](https://github.com/numpy/numpy/blob/main/numpy/__init__.pyi), and I believe we are (and should be!) using that for resolving the `numpy.array` symbol rather than looking at [`numpy/__init__.py`](https://github.com/numpy/numpy/blob/main/numpy/__init__.py). `numpy/__init__.pyi` imports `array` here, which isn't a `*` import: https://github.com/numpy/numpy/blob/a6ebba8941fb26efbdcb6d59a9f2e06ee64be858/numpy/__init__.pyi#L355. I think the only reason why we are emitting this diagnostic is because we do not understand it as being re-exported from the `numpy/__init__.pyi` stub: it is included in `__all__` [here](https://github.com/numpy/numpy/blob/a6ebba8941fb26efbdcb6d59a9f2e06ee64be858/numpy/__init__.pyi#L662) but we do not yet understand how `__all__` impacts the re-export convention. So it looks like this issue is just https://github.com/astral-sh/ty/issues/199?

---

_Comment by @sharkdp on 2025-04-06 16:26_

Oh, yes. I didn't know if it was better to put it here or in astral-sh/ty#199, since it wasn't clear to me if there were other `__all__` related things that need to be implemented. I mainly mentioned it because it sounded to me like `__all__` support was something that you thought about postponing. And unresolved-import errors are one of those Python developer-experience nightmares that I hope we can avoid wherever possible :-)

---

_Closed by @AlexWaygood on 2025-04-15 11:56_

---
