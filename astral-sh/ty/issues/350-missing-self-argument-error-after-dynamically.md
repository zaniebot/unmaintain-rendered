```yaml
number: 350
title: "\"Missing self argument\" error after dynamically replacing method"
type: issue
state: open
author: shiw-yang
labels:
  - bug
  - calls
  - attribute access
assignees: []
created_at: 2025-05-13T09:41:32Z
updated_at: 2025-08-15T15:16:41Z
url: https://github.com/astral-sh/ty/issues/350
synced_at: 2026-01-10T02:06:24Z
```

# "Missing self argument" error after dynamically replacing method

---

_Issue opened by @shiw-yang on 2025-05-13 09:41_

### Summary

In the provided example, I dynamically replace the A.f1 method with f1_replace through the f2 function. After this replacement,  ty reports an error:
No argument provided for required parameter 'self' of function 'f1' when calling a.f1(), though the code actually runs correctly.

### Steps to Reproduce:

```python

class A:
    def f1(self):
        print("f1")

    def f1_replace(self):
        print("replace")

    def f2(self):
        self.f1 = self.f1_replace


a = A() 
a.f1()  # No argument provided for required parameter `self` of function `f1`
a.f2()
a.f1()  # No argument provided for required parameter `self` of function `f1`

```

then use `ty check` show: 

```bash
$ ty check main.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[missing-argument]: No argument provided for required parameter `self` of function `f1`
  --> main.py:39:1
   |
38 | a = A()
39 | a.f1()
   | ^^^^^^
40 | a.f2()
41 | a.f1()
   |
info: Union variant `def f1(self) -> Unknown` is incompatible with this call site
info: Attempted to call union type `(bound method A.f1() -> Unknown) | (def f1(self) -> Unknown)`
info: `missing-argument` is enabled by default

error[missing-argument]: No argument provided for required parameter `self` of function `f1`
  --> main.py:41:1
   |
39 | a.f1()
40 | a.f2()
41 | a.f1()
   | ^^^^^^
   |
info: Union variant `def f1(self) -> Unknown` is incompatible with this call site
info: Attempted to call union type `(bound method A.f1() -> Unknown) | (def f1(self) -> Unknown)`
info: `missing-argument` is enabled by default

Found 2 diagnostics
```

### Version

ty 0.0.0-alpha.8

---

_Label `bug` added by @sharkdp on 2025-05-13 09:52_

---

_Label `calls` added by @sharkdp on 2025-05-13 09:52_

---

_Label `attribute access` added by @sharkdp on 2025-05-13 09:52_

---

_Comment by @sharkdp on 2025-05-13 10:07_

Thank you for reporting this. This is a bug in ty.

The problem here seems to be that we recognize the instance attribute assignment to `self.f1`, but since we don't understand the type of `self` (#159), we infer `Unknown` for the implicit instance attribute `f1`. We union that with the type of the `def f1` method definition, but fail to bind the `self` argument (because we think it's an instance attribute and don't invoke the descriptor protocol?).

---

_Assigned to @sharkdp by @sharkdp on 2025-05-21 08:35_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:00_

---

_Unassigned @sharkdp by @sharkdp on 2025-07-24 07:55_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:16_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:16_

---
