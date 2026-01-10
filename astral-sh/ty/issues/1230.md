```yaml
number: 1230
title: report an error when a type variable cannot be resolved on a call-site
type: issue
state: open
author: KotlinIsland
labels:
  - generics
assignees: []
created_at: 2025-09-22T01:27:10Z
updated_at: 2025-09-22T07:45:20Z
url: https://github.com/astral-sh/ty/issues/1230
synced_at: 2026-01-10T02:06:25Z
```

# report an error when a type variable cannot be resolved on a call-site

---

_Issue opened by @KotlinIsland on 2025-09-22 01:27_

### Summary

```py
class A[T]:
    def __init__(self, *t: T): ...

A()  # no error, expect: type variable `T@A` could not be resolved, please provide it explicitly
```

currently it becomes `Unknown` without an indication

### Version

_No response_

---

_Label `generics` added by @AlexWaygood on 2025-09-22 07:41_

---

_Comment by @AlexWaygood on 2025-09-22 07:45_

I agree we should do this.

We can't do it _right_ now because our current constraints solver is quite naïve, so there are many places where we should be solving a TypeVar but currently can't. That means that if we implemented this check now, it would lead to way too many false-positive diagnostics on user code. We should reconsider once the new constraints solver is further along (#623)

---
