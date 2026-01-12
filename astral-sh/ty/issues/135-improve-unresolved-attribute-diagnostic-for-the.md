```yaml
number: 135
title: "Improve `[unresolved-attribute]` diagnostic for the case of an invalid `__getattr__` method"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
  - attribute access
assignees: []
created_at: 2025-04-06T19:58:34Z
updated_at: 2025-05-11T08:06:11Z
url: https://github.com/astral-sh/ty/issues/135
synced_at: 2026-01-12T15:54:22Z
```

# Improve `[unresolved-attribute]` diagnostic for the case of an invalid `__getattr__` method

---

_@AlexWaygood_

For this code:

```py
class Foo:
    def __getattr__(self) -> str:
        return "foo"

Foo().attr
```

red-knot currently emits the following diagnostic:

```
error: lint:unresolved-attribute
 --> /Users/alexw/dev/experiment2/foo.py:5:1
  |
3 |         return "foo"
4 |
5 | Foo().attr
  | ^^^^^^^^^^ Type `Foo` has no attribute `attr`
  |
```

It's correct for us to emit a diagnostic here, but the diagnostic implies that the code will fail with `AttributeError`, and it doesn't say anything about the `__getattr__` method, which (if you didn't look at it too closely) you _might_ think would allow arbitrary attributes to be accessed on instances of `Foo` at runtime. In actual fact, the `__getattr__` method is invalid (it needs to take two arguments, not one), so running this code fails with `TypeError` at runtime rather than `AttributeError`:

```pytb
~/dev/experiment2 [1] % python foo.py
Traceback (most recent call last):
  File "/Users/alexw/dev/experiment2/foo.py", line 5, in <module>
    Foo().attr
TypeError: Foo.__getattr__() takes 1 positional argument but 2 were given
```

It would be great to add a subdiagnostic for this specific case pointing out that the reason why the attribute access will fail _despite_ the existence of the `__getattr__` method is that the `__getattr__` method is invalid

---

_Label `diagnostics` added by @AlexWaygood on 2025-04-06 19:58_

---

_Label `help wanted` added by @AlexWaygood on 2025-04-06 19:58_

---

_Comment by @sharkdp on 2025-04-06 20:23_

> It would be great to add a subdiagnostic for this specific case pointing out that the reason why the attribute access will fail _despite_ the existence of the `__getattr__` method is that the `__getattr__` method is invalid

We can still keep this as a separate issue, but this is essentially the goal of https://github.com/astral-sh/ty/issues/194. There are many things that can go wrong when accessing an attribute, which we would like to report as sub-diagnostics on `unresolved-attribute`. This is one particular instance. Another example would be a failed `__get__` call for a descriptor attribute. Or a missing `getter` on a `property` attribute.

---

_Comment by @AlexWaygood on 2025-04-06 20:25_

> We can still keep this as a separate issue, but this is essentially the goal of [#16298](https://github.com/astral-sh/ty/issues/194). There are many things that can go wrong when accessing an attribute, which we would like to report as sub-diagnostics on `unresolved-attribute`. This is one particular instance. Another example would be a failed `__get__` call for a descriptor attribute. Or a missing `getter` on a `property` attribute.

good point -- I've explicitly marked this as a sub-issue of astral-sh/ty#194.

---

_Label `help wanted` removed by @AlexWaygood on 2025-04-06 21:04_

---

_Label `diagnostics` added by @MichaReiser on 2025-05-07 15:19_

---

_Renamed from "[red-knot] Improve `[unresolved-attribute]` diagnostic for the case of an invalid `__getattr__` method" to "Improve `[unresolved-attribute]` diagnostic for the case of an invalid `__getattr__` method" by @MichaReiser on 2025-05-07 15:25_

---

_Label `attribute access` added by @AlexWaygood on 2025-05-11 08:06_

---
