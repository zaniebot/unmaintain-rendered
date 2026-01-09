---
number: 18372
title: "`PYI029` and `@abstractmethod` implementations"
type: issue
state: open
author: jorenham
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-05-29T15:28:25Z
updated_at: 2025-05-31T12:39:07Z
url: https://github.com/astral-sh/ruff/issues/18372
synced_at: 2026-01-07T13:12:16-06:00
---

# `PYI029` and `@abstractmethod` implementations

---

_Issue opened by @jorenham on 2025-05-29 15:28_

### Summary

```pyi
from abc import abstractmethod
from typing import override

class BaseWithCustomStr:
    @override  # overrides `builtins.__str__`
    @abstractmethod
    def __str__(self, /) -> str: ...  # ✅

class ImplWithCustomStr(BaseWithCustomStr):
    @override
    def __str__(self, /) -> str: ...  # ❌ Defining `__str__` in a stub is almost always redundant
```

But `ImplWithCustomStr.__str__` *must* be defined here. Because if omitted, `mypy` will report it as an error:

```
abstract_pyi029_repro.pyi:9: error: Class abstract_pyi029_repro.ImplWithCustomStr has abstract attributes "__str__"  [misc]
abstract_pyi029_repro.pyi:9: note: If it is meant to be abstract, add 'abc.ABCMeta' as an explicit metaclass
```

Stubtest also isn't happy about this, even if `# type: ignore[misc]`'d.

### Version

ruff 0.11.12

### PYI029 docs

https://docs.astral.sh/ruff/rules/str-or-repr-defined-in-stub/

---

_Comment by @jorenham on 2025-05-29 15:30_

Btw, I wasn't able to figure out how to configure the playground for `.pyi`. Is that not supported yet, or am I missing something?

---

_Comment by @MichaReiser on 2025-05-29 17:35_

> Btw, I wasn't able to figure out how to configure the playground for `.pyi`. Is that not supported yet, or am I missing something?

The playground unfortunately doesn't support pyi files. 

---

_Comment by @ntBre on 2025-05-29 18:40_

Makes sense, this seems like a bug to me. We could probably detect this currently within the same file, but we'll need multi-file analysis to resolve base classes from other files.

---

_Label `bug` added by @ntBre on 2025-05-29 18:40_

---

_Label `type-inference` added by @ntBre on 2025-05-29 18:40_

---

_Comment by @MichaReiser on 2025-05-29 20:08_

Could we allow str overrides that are marked with override or would this result in too many false negatives?

---

_Comment by @jorenham on 2025-05-29 20:20_

It'll solve this problem for my projects, and it might convince some others to also start using `@override`. But as far as I've seen, not many stub maintainers use `@override`; even typeshed doesn't (and are not interested in doing so: https://github.com/python/typeshed/pull/14115). 

---

_Comment by @MichaReiser on 2025-05-29 20:45_

Cc @AlexWaygood 

---

_Comment by @AlexWaygood on 2025-05-30 21:58_

Yeah, I agree that we should at least try to avoid emitting this lint on classes that override `@abstractmethod` `__str__` methods. Obviously there's only so much we can do in Ruff there right now due to the limitations of single-file analysis, but we should do what we can.

I'm not sure I see the value of special-casing `@override`-decorated methods. If a `__str__` method on a class is decorated with `@override`, all that tells you is that the type checker will enforce that some superclass of that class also has a `__str__` method. And that doesn't actually tell us anything at all, because every single class in Python has a superclass with a `__str__` method (`object` has a `__str__` method). What we want to know is whether the `__str__` method on the superclass is decorated with `@abstractmethod`, and whether or not the subclass `__str__` method is decorated with `@override` doesn't help us there unfortunately.

---
