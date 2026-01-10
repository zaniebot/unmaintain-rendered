```yaml
number: 1260
title: "`dataclass_transform`: Add support for `kw_only_default` and `frozen_default`"
type: issue
state: closed
author: rayansostenes
labels:
  - good first issue
  - dataclasses
assignees: []
created_at: 2025-09-26T09:38:32Z
updated_at: 2025-10-09T13:54:15Z
url: https://github.com/astral-sh/ty/issues/1260
synced_at: 2026-01-10T02:06:25Z
```

# `dataclass_transform`: Add support for `kw_only_default` and `frozen_default`

---

_Issue opened by @rayansostenes on 2025-09-26 09:38_

### Summary

```py
import typing as tp

@tp.dataclass_transform(kw_only_default=True, frozen_default=True)
def model(*, kw_only: bool = True):
    def decorator[T](cls: type[T]) -> type[T]:
        return cls
    return decorator

@tp.dataclass_transform(kw_only_default=True, frozen_default=True)
class ModelBase:
    def __init_subclass__(cls, *, kw_only: bool = True) -> None: ...


@tp.dataclass_transform(kw_only_default=True, frozen_default=True)
class ModelMeta(type):
    def __new__(
        cls,
        name: str,
        bases: tuple[type[tp.Any], ...],
        namespace: tp.Mapping[str, tp.Any],
        *,
        kw_only: bool = True
    ):
        ...

class ModelBase_Metaclass(metaclass=ModelMeta): ...


@model()
class A:
    name: str

@model(kw_only=False)
class B:
    name: str

class C(ModelBase):
    name: str

class D(ModelBase_Metaclass):
    name: str

class E(ModelBase_Metaclass, kw_only=False):
    name: str

tp.reveal_type(A.__init__) # ✅ `(self: A, *, name: str) -> None`
tp.reveal_type(B.__init__) # ❌ `(self: B, *, name: str) -> None` => `(self: B, name: str) -> None`
tp.reveal_type(C.__init__) # ❌ `def __init__(self) -> None`      => `(self: C, *, name: str) -> None`
tp.reveal_type(D.__init__) # ❌ `(self: D, name: str) -> None`    => `(self: D, *, name: str) -> None`
tp.reveal_type(E.__init__) # ✅ `(self: E, name: str) -> None`

def test_frozen(
    a: A,
    b: B,
    c: C,
    d: D,
    e: E,
):
    a.name = "test" # ✅ error: Property `name` defined in `A` is read-only (invalid-assignment)
    b.name = "test" # ✅ error: Property `name` defined in `B` is read-only (invalid-assignment)
    c.name = "test" # ❌ No error reported
    d.name = "test" # ❌ No error reported
    e.name = "test" # ❌ No error reported
```

**Expected**:
```console
Revealed type: `(self: A, *, name: str) -> None` (revealed-type)
Revealed type: `(self: B, name: str) -> None` (revealed-type)
Revealed type: `(self: C, *, name: str) -> None` (revealed-type)
Revealed type: `(self: D, *, name: str) -> None` (revealed-type)
Revealed type: `(self: E, name: str) -> None` (revealed-type)
Property `name` defined in `A` is read-only (invalid-assignment)
Property `name` defined in `B` is read-only (invalid-assignment)
Property `name` defined in `C` is read-only (invalid-assignment)
Property `name` defined in `D` is read-only (invalid-assignment)
Property `name` defined in `E` is read-only (invalid-assignment)
```

**Actual**:
```console
Revealed type: `(self: A, *, name: str) -> None` (revealed-type)
Revealed type: `(self: B, *, name: str) -> None` (revealed-type)
Revealed type: `def __init__(self) -> None` (revealed-type)
Revealed type: `(self: D, name: str) -> None` (revealed-type)
Revealed type: `(self: E, name: str) -> None` (revealed-type)
Property `name` defined in `A` is read-only (invalid-assignment)
Property `name` defined in `B` is read-only (invalid-assignment)
```

**Ref:**
https://typing.python.org/en/latest/spec/dataclasses.html#the-dataclass-transform-decorator


**Playground Links:**
- https://play.ty.dev/6c84c71f-33db-47e4-977d-606486daaad6
- [Pyright Playground](https://pyright-play.net/?pyrightVersion=1.1.404&strict=true&locale=en-us&code=JYWwDg9gTgLgBDAnmYA7A5nAhgZwWAWAChiABGMAOgBMsYsBjAG1xwH0YotUcAzaEAAoA1gHc2EVE0RtqAU15YArkxgBeACpQlcgDRxeUCAC85qWQuWrN2uQEpi83nBAR5TQQCp9YiVMQAXHAARhAQTHBqcFo6dgHEcIlwTslyDNB00ADaGgC6gsw4QUhgcjm5dnAAtAB8CMhlefFESa1wUHIwSlCocIUJSR1dPanpXDDQxGQUNHSMLDjsnNx8AiLiktIWiirqMXoGRqbmTlZ7tg5EzKxwALJuckwAQrhyza0pbGxowDBsOEpgtdFl8CkwcPpvHBfJtAiEwhEovtKrU4AA5SRvOCUHFTEhEchUWj0YFLLg8fhQIQw-zbM42HT6QwmMx03YM%2BzEUl3B5MW6dLCCEr2d5JT5sVBycRsQQDNp9cG6OVtVBYEBYnCcJUteUhV5FBBKMBMMrCrIzACCqEQuX0OMotuVrVV6pwYEYWJmtywYBQGCymqg%2Bkt1sdOvl3idSRp0iCoXCkWitmVcSjiXteO593cLxwcjY-JJCxwgnVRdYamzj0LWDi2Nx%2BLIrncgku3ItosSLo1nDxpGbj3WfmkagAYlhwZyrsW4E9O3Bu0FA5mZwBhQRV56vVPhhdqntQFc3AAiG95ufzNdJO%2Bd%2B6Xvfx3IAomec68CwLST4Nv4xxO8zeSSLnAy74jMHQAG5yBOHANIIFqUF8Px-GwlQAMRwIAoORwAABoIeZMLwQQWpC%2BjAYGKJ1Biko4cQ4FyFBMHCoITyId8qC-F86FwIAMuS4fhjxEbOpF7uq95QJR6KYjhkR1HhBFCU8ZF3iBnCSdRci0UQ9GMUwsGlIIq5schXFwBhfE4eKJkygp6nSfKahyQJhFBKuInkWp1RUdJdFUJB0F6cxx7GRxKHcRZzlCceyliapEleVJNGtI5-EKUE0VwFCHnxaiGlaTpAX6XIghPiFnGoWZvGpYJ6UxQedlJUkKXyTVcBPnV4kNZpeIpDAciamwzLHLKu5YMR2qtMEQRKcqDCuRNYq1cqWLtcQgGJFglDdomABEfWajtlXYXIUBGFAQQAApGKUsCILh3YyU4aByNQcBoLhFoycAeAdFg1BVLCcCCGgEETsA-2sMA6CoOqqAwJck1bfuu37TAh0Ycdp3QJd10nUg937o9CjPa9704U8X0-dB-2A8DqCg0w4NVJD0Ow-Ds1I%2BqKP9WjlV8RicAnWd7RyJAsAvcq1Cc3I3MHXzUmC1jUAi2LfXUMt0uy7z5kK0L0Aq9Aat4sQQA)



### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Label `dataclasses` added by @sharkdp on 2025-09-26 10:06_

---

_Label `good first issue` added by @sharkdp on 2025-09-26 10:06_

---

_Renamed from "`typing.dataclass_transform` Problems" to "`dataclass_transform`: Add support for `kw_only_default` and `frozen_default`" by @sharkdp on 2025-09-26 10:07_

---

_Comment by @sharkdp on 2025-09-26 10:11_

Thank you for reporting this. We do not support `kw_only_default` and `frozen_default` yet. We already record them in a bitset called `DataclassTransformerParams`, but we do not yet make use of them when creating signatures for `__init__` and when considering whether a field is frozen or not. This *might* be relatively easy to resolve (since we support the corresponding fields for normal dataclasses already), so I'm tentatively labeling this as `good-first-issue`.

---

_Closed by @sharkdp on 2025-10-09 07:34_

---

_Comment by @sharkdp on 2025-10-09 07:57_

Let's actually reopen this. We now support `kw_only_default` and `frozen_default`, but we don't apply dataclass transform params to baseclass- and metaclass-based models.

---

_Reopened by @sharkdp on 2025-10-09 07:57_

---

_Closed by @AlexWaygood on 2025-10-09 10:08_

---

_Reopened by @AlexWaygood on 2025-10-09 10:11_

---

_Comment by @sharkdp on 2025-10-09 13:54_

I have opened https://github.com/astral-sh/ty/issues/1327 as a more structured way to track the progress here. Again, thank you for reporting this!

---

_Closed by @sharkdp on 2025-10-09 13:54_

---
