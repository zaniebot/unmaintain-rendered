```yaml
number: 2232
title: No argument errors reported when a class is used as a decorator
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
assignees: []
created_at: 2025-12-26T22:55:23Z
updated_at: 2026-01-17T15:49:01Z
url: https://github.com/astral-sh/ty/issues/2232
synced_at: 2026-01-17T16:15:01Z
```

# No argument errors reported when a class is used as a decorator

---

_@MeGaGiGaGon_

### Summary

Found this while messing around, if using a class as a decorator ty does not report any errors.
https://play.ty.dev/d188d05e-e59b-44f7-8085-f180fd667424
```py
class A: ...

@A  # Should have a too-many-positional-arguments error
def foo(): ...

@int  # Should have a invalid-argument-type error
def bar(): ...
```

### Version

ty 0.0.7 (cf82a04b5 2025-12-24) playground 9693375e1

---

_Renamed from "Decorators with classes that use `__new__` do not raise argument errors" to "Decorators that use classes do not report argument errors" by @MeGaGiGaGon on 2025-12-26 22:58_

---

_Closed by @MeGaGiGaGon on 2025-12-26 23:02_

---

_Comment by @AlexWaygood on 2025-12-26 23:33_

I don't think this is a duplicate of #143, actually. That issue is about decorators being applied to classes. These examples seem to be classes being _used as decorators_, which seems pretty distinct!

---

_Reopened by @AlexWaygood on 2025-12-26 23:33_

---

_Comment by @MeGaGiGaGon on 2025-12-26 23:36_

Oops, I didn't open the example

---

_Renamed from "Decorators that use classes do not report argument errors" to "No argument errors reported when a class is used as a decorator" by @AlexWaygood on 2025-12-26 23:41_

---

_Added to milestone `Stable` by @carljm on 2025-12-27 00:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 21:00_

---

_Label `bug` added by @charliermarsh on 2026-01-04 22:25_

---

_Closed by @charliermarsh on 2026-01-17 15:49_

---
