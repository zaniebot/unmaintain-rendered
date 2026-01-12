```yaml
number: 19469
title: "[ty] Make tuple subclass constructors sound"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-sub-constructors
created_at: 2025-07-21T17:35:29Z
updated_at: 2025-07-21T21:25:12Z
url: https://github.com/astral-sh/ruff/pull/19469
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Make tuple subclass constructors sound

---

_@AlexWaygood_

## Summary

https://github.com/astral-sh/ruff/pull/18987 only partly implemented the special-casing required to make tuple instantiations sound. While that PR made instantiations of `tuple` itself sound, it did not make instantiations of tuple subclasses sound, as the way in which that PR added the special casing did not mean that the synthesized constructor signatures would be automatically inherited by subclasses. This PR fixes that by moving the special casing to a lower level, inside `ClassType::own_class_member`.

## Test Plan

Mdtests added. Some existing mdtests also had to be slightly modified because they were unsoundly constructing instances of tuple subclasses.


---

_Label `ty` added by @AlexWaygood on 2025-07-21 17:35_

---

_Comment by @github-actions[bot] on 2025-07-21 17:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-07-21 17:48_

The primer hits seem to mostly be people doing `list(map(tuple, iterable_of_iterables))`:
- https://github.com/scipy/scipy/blob/8dd9d43e62f3f6401599ced3917835d4458cece4/scipy/sparse/_construct.py#L1286
- https://github.com/scipy/scipy/blob/8dd9d43e62f3f6401599ced3917835d4458cece4/scipy/spatial/tests/test_qhull.py#L25
- https://github.com/scipy/scipy/blob/8dd9d43e62f3f6401599ced3917835d4458cece4/scipy/spatial/tests/test_qhull.py#L27
- https://github.com/more-itertools/more-itertools/blob/89172166167a5323c19ecb45fee49f191ea49bcd/more_itertools/more.py#L4164

---

_Comment by @carljm on 2025-07-21 17:52_

The new diagnostics look weird, though, because I would expect `<class 'tuple'>` to be assignable to `(Unknown, /) -> Unknown`.

---

_Comment by @AlexWaygood on 2025-07-21 17:53_

> The new diagnostics look weird, though, because I would expect `<class 'tuple'>` to be assignable to `(Unknown, /) -> Unknown`.

yeah, that's why it's still in draft :P I'm looking into it!

---

_Comment by @AlexWaygood on 2025-07-21 18:59_

Minimal repro: this still passes on this branch:

```py
from typing import Callable, Any

def f(x: Callable[..., Any]): ...

f(tuple)
```

but this no longer does:

```py
from typing import Callable, Any

def f(x: Callable[[Any], Any]): ...

f(tuple)
```

---

_Comment by @AlexWaygood on 2025-07-21 19:08_

The reason seems to be that this `.into_callable(db)` call is now returning `Callable[[], tuple[Unknown, ...]]` for `<class 'tuple'>`: https://github.com/astral-sh/ruff/blob/fcdffe4ac91daed9a227970aaf281119ce2c3007/crates/ty_python_semantic/src/types.rs#L1420-L1422

---

_Comment by @AlexWaygood on 2025-07-21 19:15_

Oh, I think the bug is just https://github.com/astral-sh/ty/issues/760 again, but for `__new__` this time rather than `__init__`

---

_Comment by @AlexWaygood on 2025-07-21 19:34_

Great, https://github.com/astral-sh/ruff/pull/19469/commits/38382034eac96277ad251a3bbbf10c89eeab1a59 fixed it! This now has a clean primer report.

---

_Marked ready for review by @AlexWaygood on 2025-07-21 19:34_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-21 19:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-21 19:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-21 19:34_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:589 on 2025-07-21 20:28_

nit: It might be worth moving this to be an instance method of `ClassLiteral`

---

_@dcreager approved on 2025-07-21 20:36_

---

_Merged by @AlexWaygood on 2025-07-21 21:25_

---

_Closed by @AlexWaygood on 2025-07-21 21:25_

---

_Branch deleted on 2025-07-21 21:25_

---
