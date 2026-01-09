---
number: 16103
title: Deny f-string
type: issue
state: open
author: danmur97
labels:
  - rule
  - type-inference
  - needs-decision
assignees: []
created_at: 2025-02-11T20:06:06Z
updated_at: 2025-02-12T14:01:40Z
url: https://github.com/astral-sh/ruff/issues/16103
synced_at: 2026-01-07T13:12:16-06:00
---

# Deny f-string

---

_Issue opened by @danmur97 on 2025-02-11 20:06_

### Description

Python f-strings are comfortable to use, but insecure. The implicit conversion to a `str` makes that changes in the type of the objects inside the expression gets unnoticed.
e.g.
say you have some URL built from an f-string `my_url = "https://something/{obj.base}/{obj.id}"` and assume `obj` is an instance of `MyObj` defined as:
```
@dataclass(frozen=True)
class MyObj:
    base: str
    obj_id: str
```
If you adjust the definition by changing the type of `obj_id` from `str` to a custom type `ObjectId` you can result in a bad encoded URL since the implicit conversion from `ObjectId` to `str` will provably have an incorrect format.

e.g. `"https://something/foo/ObjectId(obj_id='the_id')"` insead of `"https://something/foo/the_id"`

For logging and info messages this detail is worthless but for other strings that represents references (ids, urls, etc), this can cause bugs that the type checker cannot catch.

For these reasons, a rule that denies using f-strings can be useful. `"".join(["a", "b"])` or using `+` is the preferred alternative.

---

_Comment by @MichaReiser on 2025-02-12 08:32_

The concern seems valid to me, although I'm not sure if denying f-string is the way to go. To me, it seems like what you want is a rule that catches implicit `str` conversions in f-strings for type that don't have a `__str__` implementation. 

---

_Label `rule` added by @MichaReiser on 2025-02-12 08:32_

---

_Label `needs-decision` added by @MichaReiser on 2025-02-12 08:32_

---

_Comment by @danmur97 on 2025-02-12 13:35_

I agree, my concern is also with the use of `str()` and been able to use f-string safely is desirable.

---

_Label `type-inference` added by @MichaReiser on 2025-02-12 14:01_

---

_Referenced in [astral-sh/ruff#16246](../../astral-sh/ruff/issues/16246.md) on 2025-02-19 10:03_

---
