```yaml
number: 16304
title: "[red-knot] Fix descriptor `__get__` call on class objects"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-descriptor-call-on-class-objects
created_at: 2025-02-21T12:51:06Z
updated_at: 2025-02-21T14:35:45Z
url: https://github.com/astral-sh/ruff/pull/16304
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Fix descriptor `__get__` call on class objects

---

_Pull request opened by @sharkdp on 2025-02-21 12:51_

## Summary

I spotted a minor mistake in my descriptor protocol implementation where `C.descriptor` would pass the meta type (`type`) of the type of `C`  (`Literal[C]`) as the owner argument to `__get__`, instead of passing `Literal[C]` directly.

## Test Plan

New test.

---

_Review requested from @carljm by @sharkdp on 2025-02-21 12:51_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-21 12:51_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-21 12:51_

---

_Label `red-knot` added by @sharkdp on 2025-02-21 12:51_

---

_@sharkdp reviewed on 2025-02-21 12:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:305 on 2025-02-21 12:51_

This test would previously fail, since we would pass `type`, which is not assignable to `type[C]`.

---

_@AlexWaygood approved on 2025-02-21 14:16_

This LGTM.

But... looking at this specific code in more detail makes me realise that we probably aren't handling descriptors on metaclasses correctly yet -- e.g. at runtime you can do this:

```pycon
>>> class Descriptor:
...     def __get__(self, instance, owner=None):
...         if instance is None:
...             return self
...         else:
...             return 42
...             
>>> class Meta(type):
...     attr = Descriptor()
...     
>>> class UsesMeta(metaclass=Meta): ...
... 
>>> UsesMeta.attr
42
```

And in fact, it doesn't seem like we delegate attribute access from class-literal types to their metaclass at all right now 🙁 we emit a false positive even on this:

```py
class Meta(type):
    X = 42

class UsesMeta(metaclass=Meta): ...

print(UseMeta.X)  # false positive [unresolved-attribute] error from red-knot here
```

---

_Comment by @sharkdp on 2025-02-21 14:35_

> it doesn't seem like we delegate attribute access from class-literal types to their metaclass at all right now

Thanks. Would you mind opening a ticket for this? You probably have a better picture of the desired semantics.

---

_Merged by @sharkdp on 2025-02-21 14:35_

---

_Closed by @sharkdp on 2025-02-21 14:35_

---

_Branch deleted on 2025-02-21 14:35_

---
