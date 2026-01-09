---
number: 20116
title: D105 appears on overload definitions
type: issue
state: closed
author: rwb27
labels:
  - question
assignees: []
created_at: 2025-08-27T16:00:34Z
updated_at: 2025-08-28T12:48:32Z
url: https://github.com/astral-sh/ruff/issues/20116
synced_at: 2026-01-07T13:12:16-06:00
---

# D105 appears on overload definitions

---

_Issue opened by @rwb27 on 2025-08-27 16:00_

### Summary

I have some descriptor definitions where `__get__` is overloaded (because its return type is `Self` when accessed on the class, but something else otherwise). With the `D` ruleset enabled, I get a `D105` on each overload definition.

I thought the convention was that overloads didn't get individual docstrings, so I wonder if this might need some sort of exception?

```python

class MyDescriptor:

    @overload
    def __get__(self, obj: Thing, type: type | None = None) -> Value: ...  # noqa: D105

    @overload
    def __get__(self, obj: None, type: type) -> Self: ...  # noqa: D105

    def __get__(self, obj: Thing | None, type: type | None = None) -> Value | Self:
        """Return the value or the descriptor, as per `property`.

        If ``obj`` is ``None`` (i.e. the descriptor is accessed as a class attribute),
        we return the descriptor, i.e. ``self``.

        If ``obj`` is not ``None``, we return a value. To remove the need for this
        boilerplate in every subclass, we will call ``__instance_get__`` to get the
        value.

        :param obj: the `.Thing` instance to which we are attached.
        :param type: the `.Thing` subclass on which we are defined.

        :return: the value of the descriptor returned from ``__instance_get__`` when
            accessed on an instance, or the descriptor object if accessed on a class.
        """
        if obj is not None:
            return self.instance_get(obj)
        return self

---

_Comment by @ntBre on 2025-08-27 17:00_

Do you have `@overload` imported somewhere? I see some overload checking in the rule implementation, and the errors seem to clear up in the [playground](https://play.ruff.rs/acb227d1-8382-4f71-a044-c89d23fc1a90) when I add the import and remove the noqa comments in your example!

---

_Label `question` added by @ntBre on 2025-08-27 17:00_

---

_Comment by @rwb27 on 2025-08-28 08:20_

ah sorry, I didn't paste in that bit. `overload` is imported from `typing`:

```python
from typing import overload, Generic, Mapping, TypeVar, TYPE_CHECKING
```

I have just tried it again, and I think the problem is me - sorry! `D105` was at some point being checked both by ruff and by the `pydocstyle` flake8 plugin. I think I got confused, and it was the flake8 plugin that fired spuriously on the overloads. Sorry to have trespassed on your time, and thanks so much for your help.

---

_Closed by @rwb27 on 2025-08-28 08:20_

---

_Referenced in [labthings/labthings-fastapi#180](../../labthings/labthings-fastapi/pulls/180.md) on 2025-08-28 11:40_

---

_Comment by @ntBre on 2025-08-28 12:48_

No problem, glad it's working now!

---
