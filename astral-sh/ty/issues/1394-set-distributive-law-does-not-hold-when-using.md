```yaml
number: 1394
title: Set distributive law does not hold when using complements sometimes
type: issue
state: open
author: MeGaGiGaGon
labels:
  - set-theoretic types
assignees: []
created_at: 2025-10-18T06:55:09Z
updated_at: 2025-10-24T18:50:07Z
url: https://github.com/astral-sh/ty/issues/1394
synced_at: 2026-01-10T02:06:25Z
```

# Set distributive law does not hold when using complements sometimes

---

_Issue opened by @MeGaGiGaGon on 2025-10-18 06:55_

### Summary

Per the distributive law:
`A∪(B∩C)=(A∪B)∩(A∪C)`
Replacing `B` with `A'`, the distributive law should still hold, but it does not, [example](https://play.ty.dev/dfa369c0-1001-4d33-8e13-6c95fb719cca):
```py
from ty_extensions import static_assert, is_equivalent_to, Intersection, Not
from typing import Literal

class A: ...
class B: ...
class C: ...
# Works correctly (no error)
static_assert(is_equivalent_to(A | Intersection[B, C], Intersection[A | B, A | C]))
# Fails incorrectly (error)
static_assert(is_equivalent_to(A | Intersection[Not[A], C], Intersection[A | Not[A], A | C]))
```

<details>
<summary> powershell demo </summary>

```powershell
PS D:\python_projects\ty_issues> echo @'
from ty_extensions import static_assert, is_equivalent_to, Intersection, Not
from typing import Literal

class A: ...
class B: ...
class C: ...
# Works correctly (no error)
static_assert(is_equivalent_to(A | Intersection[B, C], Intersection[A | B, A | C]))
# Fails incorrectly (error)
static_assert(is_equivalent_to(A | Intersection[Not[A], C], Intersection[A | Not[A], A | C]))
'@ > main.py
PS D:\python_projects\ty_issues> uvx ty check main.py
Checking ------------------------------------------------------------ 1/1 files                                         error[static-assert-error]: Static assertion error: argument of type `ty_extensions.ConstraintSet[never]` is statically known to be falsy
  --> main.py:10:1
   |
 8 | static_assert(is_equivalent_to(A | Intersection[B, C], Intersection[A | B, A | C]))
 9 | # Fails incorrectly (error)
10 | static_assert(is_equivalent_to(A | Intersection[Not[A], C], Intersection[A | Not[A], A | C]))
   | ^^^^^^^^^^^^^^------------------------------------------------------------------------------^
   |               |
   |               Inferred type of argument is `ty_extensions.ConstraintSet[never]`
   |
info: rule `static-assert-error` is enabled by default

Found 1 diagnostic
PS D:\python_projects\ty_issues> uvx ty version
ty 0.0.1-alpha.23 (5c51b8480 2025-10-16)
```

</details>

### Version

ty 0.0.1-alpha.23 (5c51b8480 2025-10-16)

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-10-18 08:47_

---

_Comment by @sharkdp on 2025-10-18 12:33_

Thank you for reporting this.

We represent the left hand side of the lower equivalence test as `A | (C & ~A)`. The right hand side will be (correctly) simplified to `A | C` (because `A | ~A = object`). But we fail to recognize that those are the same type.

---

_Comment by @carljm on 2025-10-24 18:50_

This is similar to https://github.com/astral-sh/ty/issues/224

---
