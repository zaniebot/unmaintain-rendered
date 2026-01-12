```yaml
number: 1728
title: remove inferred bivariance
type: issue
state: open
author: carljm
labels:
  - generics
assignees: []
created_at: 2025-12-02T19:11:23Z
updated_at: 2025-12-02T19:11:23Z
url: https://github.com/astral-sh/ty/issues/1728
synced_at: 2026-01-12T15:54:25Z
```

# remove inferred bivariance

---

_@carljm_

We infer bivariance when it is sound to do so; that is, when a type variable is unused in the body of a class:

```py
class C[T]:
    pass
```

Since this type variable is unused, any specialization of `C` is equivalent to any other specialization -- it does not matter how you specialize `C`. In other words, `C` is bivariant in `T`.

So our behavior here is correct. But it is confusing, and not useful. We repeatedly run into issues in our test suite where we accidentally make a class bivariant in a typevar and then tests pass for the wrong reason, or fail unexpectedly, since bivariance is not intuitive (covariance is the intuitive expectation).

We should at least emit a diagnostic when we infer a typevar as bivariant (since it indicates the typevar is useless and could just be removed). And we should probably also go ahead and just infer the typevar as covariant in that case.

---

_Added to milestone `Stable` by @carljm on 2025-12-02 19:11_

---

_Label `generics` added by @carljm on 2025-12-02 19:11_

---
