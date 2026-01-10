```yaml
number: 17462
title: "[red-knot] fix constructor calls on classes with metaclasses"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-04-18T15:32:24Z
updated_at: 2025-04-30T15:27:10Z
url: https://github.com/astral-sh/ruff/issues/17462
synced_at: 2026-01-10T11:09:58Z
```

# [red-knot] fix constructor calls on classes with metaclasses

---

_Issue opened by @carljm on 2025-04-18 15:32_

The lookup logic for `__new__` added in #16512 is faulty. It adds "member lookup policies" to skip `object.__new__` and `type.__new__`, and then we effectively special-case handling of an implied `object.__new__` if no `__new__` is found in the lookup. But this logic doesn't work if there is a custom metaclass: we'll still find `__new__` on the custom metaclass (after not finding it on the class itself, since we skip `object.__new__`) and then we'll wrongly try to use the metaclass `__new__`:

```py
import abc

class Foo(metaclass=abc.ABCMeta): ...  

# error: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__` (lint:missing-argument) [Ln 5, Col 1]
# error: Argument to this function is incorrect: Expected `str`, found `Literal[Foo]` (lint:invalid-argument-type) [Ln 5, Col 1]
Foo()
```

We need to better represent the actual runtime lookup logic here, which is that `object.__new__` does exist, therefore `__new__` will never be looked up on the metaclass at all. One approach would be to have something like a known synthetic function type for `object.__new__`, and return that instead of not-found, and then we can recognize that known method type and special-case its usage appropriately when we get it back from the lookup. That will prevent us wrongly going to try to find `__new__` on the metaclass (and should remove the need for the skip-type lookup policy.)

Or an alternative approach might be to replace the skip-type lookup policy with an even more general never-look-up-on-the-meta-type policy.

---

_Label `red-knot` added by @carljm on 2025-04-18 15:32_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-04-18 15:32_

---

_Assigned to @sharkdp by @sharkdp on 2025-04-18 15:36_

---

_Comment by @AlexWaygood on 2025-04-18 21:22_

The original example here no longer reproduces on `main`, because https://github.com/astral-sh/ruff/commit/454ad15aee13e5ac2fc9b67ff5907658764207df added some very hacky special-casing for `ABCMeta` specifically in order to avoid a huge number of new false positives:

https://github.com/astral-sh/ruff/blob/8fe2dd5e031464630a7aebfdeecb8ecacbb3f3d2/crates/red_knot_python_semantic/src/types/class.rs#L872-L880

But it's still easy to produce these false-positive diagnostics if you use any metaclass that is not `type` or `ABCMeta`:

```py
class Meta(type):
    def __new__(mcls, name, bases, namespace, /, **kwargs):
        return super().__new__(mcls, name, bases, namespace)

class Foo(metaclass=Meta): ...

Foo()  # error: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__` (lint:missing-argument) [Ln 7, Col 1]
```

https://playknot.ruff.rs/27559171-aef7-42c0-8db0-9a16359f66e2

...And we should obviously get rid of the temporary hack as well, replacing it with a more principled solution!

---

_Comment by @mishamsk on 2025-04-22 22:11_

oops, I've missed this one. If it is not yet fixed by next week, I can work on a permanent fix

---

_Comment by @mishamsk on 2025-04-28 00:18_

@sharkdp I see you are assigned here, but per @carljm suggestion I've picked it up. I'll open a PR soon.

so far my plan is to elevate `meta_class_no_type_fallback` to just `no_meta_class_fallback`. This seems to be easier. Gut-feeling also tells me it will be more performant than introducing a whole entire `Type` variant to only represent a "magical" `object.__new__` which Carl suggested as another possible solution. I kinda like the member policy thing, and it may be valuable in the future for other special cases, so using it should be a way to go.

---

_Assigned to @mishamsk by @sharkdp on 2025-04-28 06:34_

---

_Unassigned @sharkdp by @sharkdp on 2025-04-28 06:34_

---

_Closed by @sharkdp on 2025-04-30 15:27_

---

_Closed by @sharkdp on 2025-04-30 15:27_

---
