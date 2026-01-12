```yaml
number: 847
title: Accessing private members should be an error
type: issue
state: open
author: InSyncWithFoo
labels:
  - generics
  - typing semantics
assignees: []
created_at: 2025-07-18T00:00:59Z
updated_at: 2026-01-09T01:49:05Z
url: https://github.com/astral-sh/ty/issues/847
synced_at: 2026-01-12T15:54:24Z
```

# Accessing private members should be an error

---

_@InSyncWithFoo_

Consider this example ([playground](https://play.ty.dev/9ec508e3-29af-43e2-a761-967982cf6a56)):

```python
class C[T]:
	_v: T
	def __init__(self, v: T) -> None:
		self._v = v

c = C(0)
c._v = 1  # Currently allowed
```

The assignment to `c._v` should be reported, since otherwise an instance of `C` would be considered mutable, causing `T` to be invariant instead of covariant. For consistency, I think read accesses should also be disallowed.

This issue is related to #488.

---

_Comment by @carljm on 2025-07-18 18:49_

For soundness with inferred-variance type variables, we technically only need to forbid access of private variables typed with a typevar (or some type including the typevar), which we ignored in variance inference.

There is some tension with the gradual guarantee here; apart from the issue with variance inference, this is a fairly opinionated lint rule, not a "your code will fail" type error. If it weren't for the variance issue, I definitely wouldn't consider this a candidate for an enabled-by-default rule.

---

_Comment by @carljm on 2025-08-21 19:57_

https://discuss.python.org/t/should-variance-inference-ignore-underscore-prefixed-attributes/102978

Our variance inference does consider underscore-prefixed attributes to be immutable, so for soundness we would need to at least make mutation of them an error.

I think I'm leaning towards making mutation an error but access not an error. (We currently do still consider an underscore-prefixed attribute to constrain its typevar to covariance, so allowing access is not unsound, just mutation.)

---

_Label `generics` added by @AlexWaygood on 2025-08-21 19:58_

---

_Label `typing semantics` added by @AlexWaygood on 2025-10-06 16:40_

---

_Added to milestone `Stable` by @carljm on 2026-01-09 01:49_

---
