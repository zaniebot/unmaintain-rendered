---
number: 16810
title: "Rule request: __format__ without __str__"
type: issue
state: open
author: ilius
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-03-17T15:09:14Z
updated_at: 2025-03-17T16:29:31Z
url: https://github.com/astral-sh/ruff/issues/16810
synced_at: 2026-01-10T01:22:58Z
---

# Rule request: __format__ without __str__

---

_Issue opened by @ilius on 2025-03-17 15:09_

### Summary

Let's say we have a code like this:

```py

class A:
	def __format__(self, fmt=""):
		return "Dummy format"

print(f"{A()}")

```

Which prints `Dummy format`.

Now if you run `refurb`, it suggests:

```
[FURB183]: Replace `f"{A()}"` with `str(A())`
```

But that would break the code, and produce an output like `<__main__.A object at 0x79f88dfc1dc0>`, because the class does not override `__str__` method.

Even without refurb, someone might not know or remember that f-string calls `__format__`, and change it to `str()`.

So to prevent these kinds of problems, I would suggest adding a rule to ensure if `__format__` method exists (overridden) in a class, method `__str__` should also exists, and probably just return `self.__format__()`

---

_Renamed from "Rule suggestion: __format__ without __str__" to "Rule request: __format__ without __str__" by @ilius on 2025-03-17 15:13_

---

_Label `rule` added by @MichaReiser on 2025-03-17 16:29_

---

_Label `needs-decision` added by @MichaReiser on 2025-03-17 16:29_

---
