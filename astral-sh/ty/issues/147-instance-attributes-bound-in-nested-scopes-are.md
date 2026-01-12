```yaml
number: 147
title: instance attributes bound in nested scopes are reported as unresolved
type: issue
state: closed
author: mtshiba
labels:
  - attribute access
assignees: []
created_at: 2025-03-27T04:39:56Z
updated_at: 2025-11-11T23:19:38Z
url: https://github.com/astral-sh/ty/issues/147
synced_at: 2026-01-12T15:54:22Z
```

# instance attributes bound in nested scopes are reported as unresolved

---

_@mtshiba_

### Summary

From https://github.com/astral-sh/ruff/pull/16852#issuecomment-2756606180

```python
class C:
    def __init__(self):
        def closure():
            self.a: int = 1
        closure()
        [... for self.b in range(1)]
        class D:
            self.c = 1

# should be OK
C().a
C().b
C().c
```

```sh
error: lint:unresolved-attribute
  --> test.py:10:7
   |
 8 |             self.c = 1
 9 |
10 | print(C().a)
   |       ^^^^^ Type `C` has no attribute `a`
11 | print(C().b)
12 | print(C().c)
   |

error: lint:unresolved-attribute
  --> test.py:11:7
   |
10 | print(C().a)
11 | print(C().b)
   |       ^^^^^ Type `C` has no attribute `b`
12 | print(C().c)
   |

error: lint:unresolved-attribute
  --> test.py:12:7
   |
10 | print(C().a)
11 | print(C().b)
12 | print(C().c)
   |       ^^^^^ Type `C` has no attribute `c`
   |

Found 3 diagnostics
```

### Version

red-knot 0.0.0 (640d821108b6c4ed223b4b8ca1153facb558f870)

---

_Comment by @InSyncWithFoo on 2025-03-27 06:06_

Also consider the case when an attribute is defined within a method of a class defined in `__init__`:

([playground](https://playknot.ruff.rs/5afb0a6e-4ba0-4ab5-bd78-ab99ad867b25))

```python
class C:
	def __init__(self):
		class D:
			def f(_):
				self.a = 1  # Should be reported
		self.d = D
```


---

_Renamed from "[red-knot] instance attributes bound in nested scopes are reported as unresolved" to "instance attributes bound in nested scopes are reported as unresolved" by @MichaReiser on 2025-05-07 15:25_

---

_Label `attribute access` added by @AlexWaygood on 2025-05-11 08:06_

---

_Comment by @carljm on 2025-11-11 23:19_

After https://github.com/astral-sh/ruff/pull/20856 we now support the `b` and `c` cases in the OP, but not the `a` case. At least for the moment, this is intentional. We could reconsider if we get a lot of user reports, but for now I'm going to close this as completed.

---

_Closed by @carljm on 2025-11-11 23:19_

---
