```yaml
number: 862
title: "False positive: type checker thinks `__getitem__` is required for `foo[2] = 2`"
type: issue
state: closed
author: mflova
labels:
  - bug
assignees: []
created_at: 2025-07-21T12:22:22Z
updated_at: 2025-07-30T18:34:54Z
url: https://github.com/astral-sh/ty/issues/862
synced_at: 2026-01-12T15:54:24Z
```

# False positive: type checker thinks `__getitem__` is required for `foo[2] = 2`

---

_@mflova_

### Summary

It seems the tool mixes `__getitem__` protocol with `__setitem__`. In the situation below, `ty` seems to be thinking that I am accessing the `__getitem__` method when it is `__setitem__`:

```py
class Foo:
    def __setitem__(self, key: int, value: int) -> None:
        print(f"Setting {key} to {value}")


foo = Foo()
foo[2] = 2  # Cannot subscript object of type `Foo` with no `__getitem__` method
```

### Version

ty 0.0.1-alpha.15 (0369a3598 2025-07-18)

---

_Renamed from "False positive: type checker thinks __getitem__ is required for foo[2] = 2" to "False positive: type checker thinks `__getitem__` is required for `foo[2] = 2`" by @mflova on 2025-07-21 12:22_

---

_Label `bug` added by @carljm on 2025-07-21 16:20_

---

_Comment by @carljm on 2025-07-21 16:21_

Thanks for the report! This is definitely a bug.

---

_Added to milestone `Beta` by @carljm on 2025-07-23 23:14_

---

_Comment by @MatthewMckee4 on 2025-07-26 22:00_

Help wanted? If no one's has picked it up in the next couple of days I will when I'm available, thanks

---

_Comment by @AlexWaygood on 2025-07-27 10:43_

go for it!

---

_Closed by @sharkdp on 2025-07-30 18:34_

---
