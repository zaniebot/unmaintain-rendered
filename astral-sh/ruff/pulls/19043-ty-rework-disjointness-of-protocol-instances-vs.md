```yaml
number: 19043
title: "[ty] Rework disjointness of protocol instances vs types with possibly unbound attributes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/proto-disjointness
created_at: 2025-06-30T10:39:36Z
updated_at: 2025-07-01T11:47:29Z
url: https://github.com/astral-sh/ruff/pull/19043
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Rework disjointness of protocol instances vs types with possibly unbound attributes

---

_Pull request opened by @AlexWaygood on 2025-06-30 10:39_

## Summary

Currently, we consider `Foo` and `Bar` here to be disjoint types:

```py
from typing import final, Protocol

def coinflip() -> bool:
    return False

@final
class Foo:
    if coinflip():
        x = 42

class Bar(Protocol):
    x: int
```

We reach this conclusion because:
1. We see that `Foo` is `@final`, so we therefore deduce that if `Foo` does not satisfy the protocol `Bar`, ergo no _subtype_ of `Foo` could ever satisfy the protocol `Bar`
2. We see that the `x` attribute on `Foo` is possibly unbound, meaning that `Foo` does not satisfy the protocol `Bar`, since `Bar` mandates that an object must have a definitely bound `x` attribute in order to satisfy its interface.

This has the unfortunate consequence that we infer the `hasattr()` check here as having a truthiness of `AlwaysFalse`; we believe that the branch is unreachable:

```py
def _(f: Foo):
    if hasattr(f, "x"):
        ...
```

That doesn't seem desirable. This PR reworks our protocol-disjointness logic for `@final` nominal types. Under the new logic, types which have possibly unbound attributes are still not considered subtypes of protocols that mandate those attributes. But they are also not considered _disjoint_ unless the necessary attributes either don't exist at all or exist on the nominal instance type with a disjoint type.

Similar changes are also made to module-literal types, function-literal types and class-literal types: it seems also undesirable for `hasattr()` checks involving these types to have an inferred visibility of `AlwaysFalse`.

## Test Plan

mdtests


---

_Label `ty` added by @AlexWaygood on 2025-06-30 10:39_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-30 10:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-30 10:39_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-30 10:39_

---

_Comment by @github-actions[bot] on 2025-06-30 10:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB

pydantic (https://github.com/pydantic/pydantic)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~142MB

ignite (https://github.com/pytorch/ignite)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

jinja (https://github.com/pallets/jinja)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

vision (https://github.com/pytorch/vision)
-     memo fields = ~276MB
+     memo fields = ~304MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~334MB
+     memo fields = ~304MB

```
</details>


---

_Converted to draft by @AlexWaygood on 2025-06-30 10:44_

---

_Comment by @AlexWaygood on 2025-06-30 11:14_

> ```diff
> + error[missing-argument] pydantic/_internal/_serializers.py:50:8: No argument provided for required parameter `self` of function `mode_is_json`
> ```

Here's a (bizarre) minimal repro. On `main` we allow both of the `mode_is_json()` calls here. On this branch, we allow the `f.mode_is_json()` call but disallow the `g.mode_is_json()` call:

```py
from typing import Protocol, reveal_type

class SerializationInfo1(Protocol):
    def mode_is_json(self) -> bool: ...

class SerializationInfo2(Protocol):
    def mode_is_json(self) -> bool: ...
    def __str__(self) -> str: ...

def _(f: SerializationInfo1, g: SerializationInfo2):
    reveal_type(f.mode_is_json())
    reveal_type(g.mode_is_json())
```

---

_Comment by @AlexWaygood on 2025-06-30 11:21_

Some more `reveal_type` calls:

```py
from typing_extensions import Protocol, reveal_type, TypeVar, Any, get_protocol_members

class SerializationInfo1(Protocol):
    def mode_is_json(self) -> bool: ...

class SerializationInfo2(Protocol):
    def mode_is_json(self) -> bool: ...
    def __str__(self) -> str: ...

# revealed: `frozenset[Literal["mode_is_json"]]`
reveal_type(get_protocol_members(SerializationInfo1))
# revealed: `frozenset[Literal["__str__", "mode_is_json"]]`
reveal_type(get_protocol_members(SerializationInfo2))

def _(f: SerializationInfo1, g: SerializationInfo2):
    # revealed: `bound method SerializationInfo1.mode_is_json() -> bool`
    reveal_type(f.mode_is_json)
    # revealed: `def mode_is_json(self) -> bool`
    reveal_type(g.mode_is_json)
```

I don't yet know why we're inferring a function-literal type for `g.mode_is_json` rather than a bound-method type like for `f.mode_is_json`. But it appears _related_ to the fact that `object` has a `__str__` method (they're both treated the same if the second member is a `bar` method rather than a `__str__` method).

---

_Marked ready for review by @AlexWaygood on 2025-07-01 11:24_

---

_Comment by @AlexWaygood on 2025-07-01 11:25_

The weirdness in the original primer output is now fixed -- there was a bug in the PR (I was using `.all()` instead of `.any()`...) that interacted with a pre-existing bug on `main` (https://github.com/astral-sh/ty/issues/737) to produce some very perplexing results -- many thanks to @sharkdp for helping me track that down!

This PR is now ready for review.

---

_@sharkdp approved on 2025-07-01 11:41_

Thank you!

---

_Merged by @AlexWaygood on 2025-07-01 11:47_

---

_Closed by @AlexWaygood on 2025-07-01 11:47_

---

_Branch deleted on 2025-07-01 11:47_

---
