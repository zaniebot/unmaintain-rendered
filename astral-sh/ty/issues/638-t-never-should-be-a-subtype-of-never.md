```yaml
number: 638
title: "`T: Never` should be a subtype of `Never`"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - help wanted
  - generics
  - type properties
assignees: []
created_at: 2025-06-12T04:55:05Z
updated_at: 2025-06-16T17:46:18Z
url: https://github.com/astral-sh/ty/issues/638
synced_at: 2026-01-12T15:54:23Z
```

# `T: Never` should be a subtype of `Never`

---

_@dhruvmanila_

### Summary

Currently, the `Type::has_relation_to` has hardcoded the fact that no fully static type other than `Never` is a subtype of `Never` but that's incorrect because a type variable `T` with upper bound `Never` should also be a subtype of `Never`.

Playground: https://play.ty.dev/9b734d22-6fdf-4f20-a357-eea12caa5336

```py
from typing import Never
from ty_extensions import static_assert, is_subtype_of

def _[T: Never]():
    # This is false, but should be true
    static_assert(is_subtype_of(T, Never))
```

### Version

_No response_

---

_Label `bug` added by @dhruvmanila on 2025-06-12 04:55_

---

_Label `generics` added by @dhruvmanila on 2025-06-12 04:55_

---

_Label `help wanted` added by @carljm on 2025-06-12 05:10_

---

_Label `type properties` added by @dhruvmanila on 2025-06-12 05:13_

---

_Closed by @AlexWaygood on 2025-06-16 17:46_

---
