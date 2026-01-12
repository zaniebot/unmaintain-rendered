```yaml
number: 15959
title: "[red-knot] Extend instance/class attribute tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/extend-attribute-tests
created_at: 2025-02-05T10:32:05Z
updated_at: 2025-02-05T16:37:43Z
url: https://github.com/astral-sh/ruff/pull/15959
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Extend instance/class attribute tests

---

_@sharkdp_

## Summary

In preparation for creating some (sub) issues for https://github.com/astral-sh/ruff/issues/14164, I'm trying to document the current behavior (and a bug) a bit better.


---

_Label `testing` added by @sharkdp on 2025-02-05 10:32_

---

_Label `red-knot` added by @sharkdp on 2025-02-05 10:32_

---

_Review requested from @carljm by @sharkdp on 2025-02-05 10:32_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-05 10:32_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-05 10:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:236 on 2025-02-05 10:36_

Since we'll have emitted a diagnostic above complaining about the redeclaration, I think the current behaviour here is also reasonable TBH. I wouldn't worry too much about inferring a union here if it complicates the implementation.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:491 on 2025-02-05 10:39_

interesting... do mypy/pyright support narrowing like this, where you narrow it on the class but then read it from an instance?

---

_@AlexWaygood approved on 2025-02-05 10:39_

---

_@sharkdp reviewed on 2025-02-05 10:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:378 on 2025-02-05 10:43_

This might be difficult to support(?).

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:391 on 2025-02-05 10:43_

Pyright revals `int` here, which is wrong.

---

_@sharkdp reviewed on 2025-02-05 10:44_

---

_@sharkdp reviewed on 2025-02-05 10:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:491 on 2025-02-05 10:50_

Mypy doesn't seem to support this detection of implicit `pure_class_variable`s at all. Pyright supports narrowing in the case above (`C.pure_class_variable`), but not here.

Maybe I should remove the TODO here?

---

_@AlexWaygood reviewed on 2025-02-05 10:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:491 on 2025-02-05 10:57_

Yeah, I don't think this is something we "have" to support -- I don't think it's something users will expect of us. (I think they _will_ expect attribute narrowing when the attribute is accessed on the same variable, such as `C.pure_class_variable` above.) Obviously if it's easy and naturally falls out of the implementation, then that's fantastic -- I think it isn't any more unsound than attribute narrowing in general. But I don't think it's something we need a TODO for.

---

_@AlexWaygood reviewed on 2025-02-05 10:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:391 on 2025-02-05 10:58_

oof. might be worth a bug report to pyright!

---

_@AlexWaygood reviewed on 2025-02-05 11:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:378 on 2025-02-05 11:02_

Is it also worth testing something like

````md
`somewhere_else.py`:

```py
def staticmethod(func): ...
```

`mod.py`:

```py
from somewhere_else import staticmethod

class Foo:
    @staticmethod
    def just_a_normal_method(self):
        self.x = 42

f = Foo()
reveal_type(f.x)  # revealed: int
```
````
?

---

_@AlexWaygood reviewed on 2025-02-05 11:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:378 on 2025-02-05 11:07_

and perhaps also the fully-qualified form:

```py
import builtins

class Foo:
    @builtins.staticmethod
    def this_is_a_staticmethod(other: Other):
        other.x = 42

f = Foo()
reveal_type(f.x)  # error + `revealed: Unknown`
```

---

_@sharkdp reviewed on 2025-02-05 11:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:391 on 2025-02-05 11:31_

https://github.com/microsoft/pyright/issues/9820

---

_Merged by @sharkdp on 2025-02-05 11:45_

---

_Closed by @sharkdp on 2025-02-05 11:45_

---

_Branch deleted on 2025-02-05 11:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:391 on 2025-02-05 16:37_

I won't be surprised if aliasing `staticmethod` is just considered out-of-scope for pyright to handle. I will be very happy if we can handle it, however!

---

_@carljm reviewed on 2025-02-05 16:37_

---
