```yaml
number: 636
title: "Confusing naming `unbound` for methods"
type: issue
state: closed
author: Tishka17
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-06-11T14:30:13Z
updated_at: 2025-09-23T14:26:57Z
url: https://github.com/astral-sh/ty/issues/636
synced_at: 2026-01-10T02:06:24Z
```

# Confusing naming `unbound` for methods

---

_Issue opened by @Tishka17 on 2025-06-11 14:30_

### Summary

In python there are bound methods which is actually a function/classmethod bound to an object (`<bound method A.foo of <class '__main__.A'>>`).
As on opposite, there are unbound methods - those, who were not bound.

You can find this term in that meaning here:
https://docs.python.org/3/library/unittest.mock-examples.html#mocking-unbound-methods
https://docs.python.org/3/reference/datamodel.html
https://docs.python.org/3/library/types.html

Ty uses "unbound method" as a synonym of an absent/not assigned attribute.
https://github.com/astral-sh/ty/blob/main/docs/reference/rules.md#possibly-unbound-implicit-call
> Checks for implicit calls to possibly unbound methods.

This doesn't relate to unbound local variable. 

### Version

_No response_

---

_Renamed from "Confusing naming `unbound`" to "Confusing naming `unbound` for methods" by @Tishka17 on 2025-06-11 14:30_

---

_Label `diagnostics` added by @AlexWaygood on 2025-06-11 14:31_

---

_Comment by @Tishka17 on 2025-06-11 14:34_

Probably we should avoid such usage of a term `unbound` with anything except local variables. 

---

_Label `help wanted` added by @carljm on 2025-06-11 16:20_

---

_Comment by @carljm on 2025-06-11 16:23_

I think this could be something good for a contributor to tackle; we should just review usage of the word "unbound" in all diagnostic codes and messages and consider alternative wording. If someone picks this up but isn't sure what wording to use, feel free to post questions here. I think maybe we could substitute the word "missing" for "unbound" in a lot of these cases (e.g. I think maybe `possibly-unbound-implicit-call` could be `possibly-missing-method` instead?)

---

_Comment by @theammir on 2025-06-28 22:47_

> I think maybe we could substitute the word "missing" for "unbound" in a lot of these cases (e.g. I think maybe `possibly-unbound-implicit-call` could be `possibly-missing-method` instead?)

What about "possibly undeclared"? I'm not sure if it's technically correct, however "missing" doesn't convey the idea of "is only conditionally valid" to me.

---

_Comment by @carljm on 2025-09-12 21:37_

> What about "possibly undeclared"

I don't think we should use this phrasing, as ty uses the term "declared" to mean "given an explicit type annotation", while these errors are really talking about whether the attribute exists at all. Attributes can exist without being declared, e.g.

```py
class C:
    def __init__(self):
        self.x = 1
```

Instances of `C` have an `x` attribute which is definitely "bound" (definitely exists), but is not "declared".

> "missing" doesn't convey the idea of "is only conditionally valid" to me

Not sure I follow here. The issue (in the case of `possibly-unbound-implicit-call`) is that a certain dunder method might not exist. "Possibly missing" seems like a clear way to describe this?

---

_Closed by @AlexWaygood on 2025-09-23 14:26_

---
