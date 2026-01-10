```yaml
number: 256
title: Panics for self-referential (recursive) generic classes or type aliases
type: issue
state: closed
author: davidbgk
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-05-07T20:10:43Z
updated_at: 2025-11-26T16:50:28Z
url: https://github.com/astral-sh/ty/issues/256
synced_at: 2026-01-10T01:58:59Z
```

# Panics for self-referential (recursive) generic classes or type aliases

---

_Issue opened by @davidbgk on 2025-05-07 20:10_

When running it against https://github.com/numerique-gouv/ami-notifications-api @ f9fa495fcbad07244f9f7598ac47574c3ae99ef2

```
$ uvx ty check
Installed 1 package in 4ms
error: panic: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/f78a641/src/function/execute.rs:210:25 when checking `/Users/david/Sites/ami-notifications-api/tests/conftest.py`: `infer_definition_types(Id(2b01f)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D we'd be very appreciative!
info: Platform: macos x86_64
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: symbol_by_id(Id(804b))
             at crates/ty_python_semantic/src/symbol.rs:576
   1: member_lookup_with_policy_(Id(6467))
             at crates/ty_python_semantic/src/types.rs:537
   2: infer_definition_types(Id(1a9e))
             at crates/ty_python_semantic/src/types/infer.rs:147
   3: symbol_by_id(Id(804a))
             at crates/ty_python_semantic/src/symbol.rs:576
   4: infer_deferred_types(Id(1aad))
             at crates/ty_python_semantic/src/types/infer.rs:185
   5: signature_(Id(a420))
             at crates/ty_python_semantic/src/types.rs:6611
   6: infer_expression_types(Id(3009))
             at crates/ty_python_semantic/src/types/infer.rs:221
   7: infer_definition_types(Id(281d))
             at crates/ty_python_semantic/src/types/infer.rs:147
   8: infer_scope_types(Id(2404))
             at crates/ty_python_semantic/src/types/infer.rs:120
   9: check_types(Id(c01))
             at crates/ty_python_semantic/src/types.rs:83
```

When runnning it with `RUST_BACKTRACE=1` as proposed:

<details>

```
$ RUST_BACKTRACE=1 uvx ty check
error: panic: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/f78a641/src/function/execute.rs:210:25 when checking `/Users/david/Sites/ami-notifications-api/tests/conftest.py`: `infer_definition_types(Id(2b883)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D we'd be very appreciative!
info: Platform: macos x86_64
info: Args: ["ty", "check"]
info: Backtrace:
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
  17: __mh_execute_header
  18: __mh_execute_header
  19: __mh_execute_header
  20: __mh_execute_header
  21: __mh_execute_header
  22: __mh_execute_header
  23: __mh_execute_header
  24: __mh_execute_header
  25: __mh_execute_header
  26: __mh_execute_header
  27: __mh_execute_header
  28: __mh_execute_header
  29: __mh_execute_header
  30: __mh_execute_header
  31: __mh_execute_header
  32: __mh_execute_header
  33: __mh_execute_header
  34: __mh_execute_header
  35: __mh_execute_header
  36: __mh_execute_header
  37: __mh_execute_header
  38: __mh_execute_header
  39: __mh_execute_header
  40: __mh_execute_header
  41: __mh_execute_header
  42: __mh_execute_header
  43: __mh_execute_header
  44: __mh_execute_header
  45: __mh_execute_header
  46: __mh_execute_header
  47: __mh_execute_header
  48: __mh_execute_header
  49: __mh_execute_header
  50: __mh_execute_header
  51: __mh_execute_header
  52: __mh_execute_header
  53: __mh_execute_header
  54: __mh_execute_header
  55: __mh_execute_header
  56: __mh_execute_header
  57: __mh_execute_header
  58: __mh_execute_header
  59: __mh_execute_header
  60: __mh_execute_header
  61: __mh_execute_header
  62: __mh_execute_header
  63: __mh_execute_header
  64: __mh_execute_header
  65: __mh_execute_header
  66: __mh_execute_header
  67: __mh_execute_header
  68: __mh_execute_header
  69: __mh_execute_header
  70: __mh_execute_header
  71: __mh_execute_header
  72: __mh_execute_header
  73: __mh_execute_header
  74: __mh_execute_header
  75: __mh_execute_header
  76: __mh_execute_header
  77: __mh_execute_header
  78: __mh_execute_header
  79: __mh_execute_header
  80: __mh_execute_header
  81: __mh_execute_header
  82: __mh_execute_header
  83: __mh_execute_header
  84: __mh_execute_header
  85: __mh_execute_header
  86: __mh_execute_header
  87: __mh_execute_header
  88: __mh_execute_header
  89: __mh_execute_header
  90: __mh_execute_header
  91: __mh_execute_header
  92: __mh_execute_header
  93: __mh_execute_header
  94: __mh_execute_header
  95: __mh_execute_header
  96: __mh_execute_header
  97: __mh_execute_header
  98: __mh_execute_header
  99: __mh_execute_header
 100: __mh_execute_header
 101: __mh_execute_header
 102: __mh_execute_header
 103: __mh_execute_header
 104: __mh_execute_header
 105: __mh_execute_header
 106: __mh_execute_header
 107: __mh_execute_header
 108: __pthread_start

info: query stacktrace:
   0: symbol_by_id(Id(e84b))
             at crates/ty_python_semantic/src/symbol.rs:576
   1: member_lookup_with_policy_(Id(8072))
             at crates/ty_python_semantic/src/types.rs:537
   2: infer_definition_types(Id(329e))
             at crates/ty_python_semantic/src/types/infer.rs:147
   3: symbol_by_id(Id(e84a))
             at crates/ty_python_semantic/src/symbol.rs:576
   4: infer_deferred_types(Id(32ad))
             at crates/ty_python_semantic/src/types/infer.rs:185
   5: signature_(Id(2181c))
             at crates/ty_python_semantic/src/types.rs:6611
   6: infer_expression_types(Id(4009))
             at crates/ty_python_semantic/src/types/infer.rs:221
   7: infer_definition_types(Id(281d))
             at crates/ty_python_semantic/src/types/infer.rs:147
   8: infer_scope_types(Id(1404))
             at crates/ty_python_semantic/src/types/infer.rs:120
   9: check_types(Id(c01))
             at crates/ty_python_semantic/src/types.rs:83
```

</details>

Hope it helps, I'm definitely not a Rust developer (yet!).

---

_Comment by @zanieb on 2025-05-07 20:15_

Thank you! It's a known issue that we don't converge on all cycles yet, someone with deeper knowledge will determine why this particular cycle doesn't :)

---

_Label `bug` added by @carljm on 2025-05-08 04:36_

---

_Comment by @my1e5 on 2025-05-08 11:36_

Also getting this error. Can't share the source code or make a MRE right now. But perhaps it is helpful to share the logs with `RUST_BACKTRACE=1`

<details>

```
ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

$ RUST_BACKTRACE=1 uvx ty check 
error: panic: Panicked at C:\Users\runneradmin\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\f78a641\src\function\execute.rs:210:25 when checking `C:\Users\...\main.py`: `exported_names(Id(b827)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D we'd be very appreciative!
info: Platform: windows x86_64
info: Args: ["C:\\Users\\my1e5\\AppData\\Local\\uv\\cache\\archive-v0\\cS8sJwf4-jYpRSW0EuOLM\\Scripts\\ty.exe", "check"]
info: Backtrace:
   0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: <unknown>
  28: <unknown>
  29: <unknown>
  30: <unknown>
  31: <unknown>
  32: <unknown>
  33: <unknown>
  34: <unknown>
  35: <unknown>
  36: <unknown>
  37: <unknown>
  38: <unknown>
  39: <unknown>
  40: <unknown>
  41: <unknown>
  42: <unknown>
  43: <unknown>
  44: <unknown>
  45: <unknown>
  46: <unknown>
  47: <unknown>
  48: <unknown>
  49: <unknown>
  50: <unknown>
  51: <unknown>
  52: <unknown>
  53: <unknown>
  54: <unknown>
  55: <unknown>
  56: <unknown>
  57: <unknown>
  58: <unknown>
  59: <unknown>
  60: <unknown>
  61: <unknown>
  62: <unknown>
  63: <unknown>
  64: <unknown>
  65: <unknown>
  66: <unknown>
  67: <unknown>
  68: <unknown>
  69: <unknown>
  70: <unknown>
  71: <unknown>
  72: <unknown>
  73: <unknown>
  74: <unknown>
  75: <unknown>
  76: <unknown>
  77: <unknown>
  78: <unknown>
  79: <unknown>
  80: <unknown>
  81: <unknown>
  82: <unknown>
  83: <unknown>
  84: <unknown>
  85: <unknown>
  86: <unknown>
  87: <unknown>
  88: BaseThreadInitThunk
  89: RtlUserThreadStart

info: query stacktrace:
   0: semantic_index(Id(b827))
             at crates\ty_python_semantic\src\semantic_index.rs:49
   1: global_scope(Id(b827))
             at crates\ty_python_semantic\src\semantic_index.rs:138
   2: member_lookup_with_policy_(Id(15c1c))
             at crates\ty_python_semantic\src\types.rs:537
   3: infer_definition_types(Id(41c3a))
             at crates\ty_python_semantic\src\types\infer.rs:147
   4: symbol_by_id(Id(1dc17))
             at crates\ty_python_semantic\src\symbol.rs:576
   5: member_lookup_with_policy_(Id(15c1b))
             at crates\ty_python_semantic\src\types.rs:537
   6: infer_expression_types(Id(7c5a))
             at crates\ty_python_semantic\src\types\infer.rs:221
   7: infer_scope_types(Id(6c63))
             at crates\ty_python_semantic\src\types\infer.rs:120
   8: check_types(Id(c06))
             at crates\ty_python_semantic\src\types.rs:83
```

</details>

---

_Assigned to @sharkdp by @sharkdp on 2025-05-08 13:36_

---

_Comment by @sharkdp on 2025-05-08 13:57_

> When running it against https://github.com/numerique-gouv/ami-notifications-api @ f9fa495fcbad07244f9f7598ac47574c3ae99ef2

I narrowed this down to the following example which appears in a 3rd party dependency ([`litestar/middleware/session/base.py`](https://github.com/litestar-org/litestar/blob/main/litestar/middleware/session/base.py). The original names of the classes were `BaseBackendConfig` and `BaseSessionBackend`:
```py
from typing import (
    Generic,
    TypeVar,
)

T = TypeVar("T", bound="B")

class A(Generic[T]):
    pass


S = TypeVar("S", bound="A")

class B(Generic[S]):
    pass
```

The same cycle also appears if just a single class is involved:
```py
class A[T: "A"]:
    pass
```

Similar cycles (in `infer_scope_types` instead of `infer_definition_types`) also appear with self-referential type aliases like
```py
type C = dict[str, "C"]
```

Another example from https://github.com/astral-sh/ty/issues/271, which leads to a "dependency graph cycle" panic because there's no cycle recovery on `value_type`:
```py
type Foo = str | list[Foo]

f: Foo 
```

Another example from #91 which leads to a cycle on `pep695_generic_context_`
```py
class Node[T: "Node[int]"]:
    pass
```

---

_Renamed from "[panic] too many cycle iterations" to "Panics for self-referential generic classes or type aliases" by @sharkdp on 2025-05-08 14:03_

---

_Comment by @AlexWaygood on 2025-05-08 14:13_

https://github.com/astral-sh/ty/issues/91 is also the same issue as this; we should probably close either this one or that one

---

_Comment by @nickdrozd on 2025-05-08 17:53_

Here is a triggering example without any generics:

```python
from __future__ import annotations

type IntLike = int | Add

class Add:
    l: IntLike
    r: IntLike

    def __new__(cls, l: IntLike, r: IntLike) -> IntLike:
        return (
            l if r == 0 else
            r if l == 0 else
            super().__new__(cls)
        )

    def __init__(self, l: IntLike, r: IntLike):
        self.l = l
        self.r = r

    def __repr__(self) -> str:
        return f'({self.l} + {self.r})'

a0 = Add(0, 5)
a1 = Add(1, 5)

print(a0, type(a0))
print(a1, type(a1))

assert isinstance(a0, int)
assert isinstance(a1, Add)
```

Code runs fine. But Ty fails with error similar to above.

The error appears to come from line `assert isinstance(a1, Add)`. If that line is commented out, there is no error.

---

_Label `fatal` added by @AlexWaygood on 2025-05-10 17:59_

---

_Renamed from "Panics for self-referential generic classes or type aliases" to "Panics for self-referential (recursive) generic classes or type aliases" by @AlexWaygood on 2025-05-16 16:22_

---

_Comment by @yorickvP on 2025-05-18 07:31_

Tiny example:
```python
X = dict[str, "list[str | X]"]
def x() -> X:
    return {}
```

---

_Comment by @sharkdp on 2025-05-19 10:20_

The example from #297 seems worth mentioning here again, as it does not panic with "too many cycle iterations", but "dependency graph cycle when querying value_type_(Id(3400)), set cycle_fn/cycle_initial to fixpoint iterate" instead. It might be worth adding that to the eventual test corpus
```py
from __future__ import annotations

type Json = dict[str, Json] | list[Json] | str | int | float | bool | None


def parse_json(j: Json) -> Json:
    if isinstance(j, dict):
        return {k: parse_json(v) for k, v in j.items()}
    elif isinstance(j, list):
        return [parse_json(i) for i in j]
    else:
        return


print(parse_json({"a": {"b": "c"}}))
```

---

_Comment by @sharkdp on 2025-05-27 10:05_

The following example seems worth mentioning, as it produces a hang (one thread at 100% cpu, but seemingly no increase in memory usage, no stack overflow)
```py
from __future__ import annotations

type Expression = int | tuple[Expression, Expression]
```

---

_Comment by @carljm on 2025-05-27 13:09_

I suspect that part of the fix here will have to be resolving type aliases lazily rather than eagerly when they appear in type expressions. (That is, treating them as first class opaque types and adding handling for them to type relation methods.) That will mean we can't eagerly simplify some unions/intersections involving type aliases as well, but that may be ok?

---

_Comment by @AlexWaygood on 2025-05-27 13:25_

Something that's quite interesting is that mypy, like we do currently, does actually resolve type aliases eagerly to their values -- and it is still able to support recursive type aliases. So I don't think treating type aliases as first-class opaque types is _necessary_ for a type checker to support recursive type aliases (though maybe it is with our model?).

Treating them as first-class opaque types will probably make it easier whatever the case, though, and I think it's desirable anyway. If you have code like this ([adapted from typeshed](https://github.com/python/typeshed/blob/b01e7aa9d24014855c3d45b7d671a7f6b427b4c2/stdlib/_typeshed/__init__.pyi#L181C1-L217)):

`foo.py`:

```py
from typing import Literal

type OpenTextModeUpdating = Literal[
    "r+",
    "+r",
    "rt+",
    "r+t",
    "+rt",
    "tr+",
    "t+r",
    "+tr",
    "w+",
    "+w",
    "wt+",
    "w+t",
    "+wt",
    "tw+",
    "t+w",
    "+tw",
    "a+",
    "+a",
    "at+",
    "a+t",
    "+at",
    "ta+",
    "t+a",
    "+ta",
    "x+",
    "+x",
    "xt+",
    "x+t",
    "+xt",
    "tx+",
    "t+x",
    "+tx",
]

type OpenTextModeWriting = Literal["w", "wt", "tw", "a", "at", "ta", "x", "xt", "tx"]
type OpenTextModeReading = Literal["r", "rt", "tr", "U", "rU", "Ur", "rtU", "rUt", "Urt", "trU", "tUr", "Utr"]
type OpenTextMode = OpenTextModeUpdating | OpenTextModeWriting | OpenTextModeReading
```

`bar.py`:

```py
from foo import OpenTextMode

def f(x: OpenTextMode):
    reveal_type(x)
```

...I think you _really_ just want the type checker to reveal the meaningful type alias name (`OpenTextMode`, or `foo.OpenTextMode`) rather than revealing a very, very long `Literal` type. That feature should fall out naturally if we treat type aliases as first-class opaque types rather than eagerly resolving them. It's a feature mypy [has wanted for a while](https://github.com/python/mypy/issues/2968), but it doesn't seem like it's ever been high-priority enough for it to land at mypy.

---

_Comment by @sharkdp on 2025-05-27 13:49_

Recursive type-references/cycles can not just lead to panics and hangs, but also to stack overflows. Like in this recursive protocol cycle:
```py
from __future__ import annotations
from typing import Protocol

class Other(Protocol):
    other: int

class A(Protocol):
    b: B            # A links to B
    other: int

class B(Protocol):
    a: list[A]      # B links to A

def _(a: A):
    o: Other = a  # some assignment that triggers a lookup of members
```

Edit: this is the same as #93 

---

_Comment by @carljm on 2025-05-27 18:23_

Another example case, from https://github.com/astral-sh/ruff/pull/17602:

```py
class Foo[T: Foo, U: (Foo, Foo)]:
    pass
```

---

_Comment by @anmorgunov on 2025-06-06 02:46_

another one line example

```py
PaRoutesDict = dict[str, str | bool | list["PaRoutesDict"]]
```

---

_Assigned to @carljm by @carljm on 2025-06-10 23:59_

---

_Unassigned @sharkdp by @carljm on 2025-06-10 23:59_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:00_

---

_Comment by @sharkdp on 2025-07-03 08:48_

https://github.com/astral-sh/ty/issues/709#issuecomment-3031291761 has a slight variation that leads to a hang, not a panic:

```py
from __future__ import annotations

type NestedLength = int | tuple[NestedLength, ...]

n: NestedLength  # remove this to turn it from a panic into a hang
```

---

_Comment by @marcelroed on 2025-07-31 17:23_

Is a fix for this planned, or is there a WIP? I'm seeing this issue everywhere in extensively typed code, and it causes a panic whenever I import from such a package. Here's an example:

```python3
from typing import Generic, TypeVar

LmConfigT = TypeVar("LmConfigT", bound="LmConfig")
LmT = TypeVar("LmT", bound="LmHeadModel")

class LmHeadModel(Generic[LmConfigT]):
    ...

class LmConfig(Generic[LmT]):
    ...
```

---

_Comment by @AlexWaygood on 2025-07-31 17:29_

A fix is in progress currently and should be up soon, hopefully!

---

_Comment by @marcelroed on 2025-08-20 00:14_

@AlexWaygood Any updates on this? Is there a PR with a candidate solution I can build from source? Maybe a stop-gap solution could be to fall back to `Any` when a loop is detected. Currently, the panic means my IDE fails all LSP operations whenever I use a package that has this pattern (this is currently the case for a majority of my projects). Thanks!

---

_Comment by @nroman-openai on 2025-08-20 00:21_

An example I'm seeing from a third-party dep ([statsig](https://github.com/statsig-io/python-sdk/blob/987276d81651b45ffc8f1549c7d5d5c05991713d/statsig/utils.py#L9-L10)): https://play.ty.dev/57e1af15-110f-4baa-9aa4-ae7218861f0d

```
from typing import Union, TypeAliasType, Sequence, Mapping
JSONPrimitive = Union[str, int, float, bool, None]
JSONValue = TypeAliasType("JSONValue", 'Union[JSONPrimitive, Sequence["JSONValue"], Mapping[str, "JSONValue"]]')
```

We could probably hack around this if needed but wanted to be sure the repro was included.

---

_Comment by @carljm on 2025-08-20 00:37_

I'm working on a fix that will cover the cases mentioned here, including the newest one.

---

_Comment by @edaniels on 2025-08-20 00:43_

> I'm working on a fix that will cover the cases mentioned here, including the newest one.

Thank you! This will be a nice un-blocker

---

_Comment by @aidandj on 2025-08-27 16:34_

Should it be possible to exclude a file that is causing this panic? When I add the file that has a TypeVar that causes this panic to the exclude list, i'm still getting the panic.

---

_Comment by @carljm on 2025-08-27 16:37_

Exclude just means we won't emit diagnostics on it, but we'll still process it if its imported by other files. A more effective temporary workaround would be to shadow the file with a type stub (`.pyi` with the same name) that has a module-level `__getattr__` returning `Any`.

---

_Comment by @aidandj on 2025-08-27 17:24_

Hmmm, maybe I misunderstood, but I added the following `test.pyi` next to the `test.py` that was causing issues. Still getting the panic.

```py
from typing import Any

# Workaround for https://github.com/astral-sh/ty/issues/256
def __getattr__(name: str) -> Any: ...
```

and `test.py`

```py
from __future__ import annotations

from typing import TYPE_CHECKING, Generic, TypeVar

if TYPE_CHECKING:
    pass

B = TypeVar("B", bound="Base")


class Base(Generic[B]):
    a: int

    @classmethod
    def to_dict(cls, sql: B) -> dict:
        return {"a": sql.a}


class Child(Base):
    b: str

    @classmethod
    def to_dict(cls, sql: Child) -> dict:
        base_dict = super().to_dict(sql)
        base_dict.update({"b": sql.b})
        return base_dict

```

---

_Comment by @carljm on 2025-08-27 17:29_

It's possible you might need to do both the pyi and the exclude?

Or instead of the pyi you could just have `Base: Any` and `Child: Any` in the `if TYPE_CHECKING` block and the real code in the `else`.

Or just wait a week or so :)

---

_Comment by @AlexWaygood on 2025-09-16 10:39_

Another interesting repro by @chrreisinger from https://github.com/astral-sh/ty/issues/1191:

```py
from collections.abc import Sequence

type TYPE = Foo | Sequence[TYPE]


class Foo[T = TYPE]:
    bitfields: Sequence[T]
```

---

_Comment by @carljm on 2025-09-17 00:20_

With https://github.com/astral-sh/ruff/pull/20359 many of the issues reported here should be fixed. (Not all, thus not closing this issue yet -- if you have a code example here that still fails, don't worry, we're still working on it.)

---

_Comment by @aidandj on 2025-09-27 17:30_

[This PR](https://github.com/astral-sh/ruff/pull/20598) fixed my problem! Woot. Keeping an eye out for a release. Thanks guys.

---

_Comment by @AlexWaygood on 2025-10-13 14:02_

Another example from https://github.com/astral-sh/ty/issues/1347, which still repros on `main`:

```py
type A[T] = type[T] | tuple[A[T]]
```

---

_Comment by @carljm on 2025-10-14 15:52_

> Another example from [#1347](https://github.com/astral-sh/ty/issues/1347), which still repros on `main`:
> 
> type A[T] = type[T] | tuple[A[T]]

Just to note, this is the same case that is failing in the conformance suite that I was already looking at. I think we will need the ability to store lazily-applied specializations on a type-alias type. (There could also be a shorter-term fix where we use `Type::Divergent` to avoid the panic, but this wouldn't give full support for the recursive alias.)

---

_Assigned to @ibraheemdev by @MichaReiser on 2025-10-17 14:46_

---

_Unassigned @carljm by @MichaReiser on 2025-10-17 14:46_

---

_Comment by @AlexWaygood on 2025-10-17 19:20_

Following https://github.com/astral-sh/ruff/pull/20598, we no longer panic on this:

```py
from typing import TypeVar, Generic, Any

T = TypeVar("T", bound="Foo[Any]")

class Foo(Generic[T]): ...
```

Unfortunately, however, it seems we still panic on this:

```py
from typing import TypeVar, Generic

T = TypeVar("T", bound="Foo[int]")

class Foo(Generic[T]): ...
```

---

_Comment by @MichaReiser on 2025-10-24 13:28_

https://github.com/astral-sh/ruff/pull/21059 now adds a way to add "panicking" mdtests. That means, we could start adding some of those cases to our test suite now (there's one caveat, tests that panic with too many iterations can be slow, depending on how many queries are involved)

---

_Assigned to @carljm by @MichaReiser on 2025-11-10 16:26_

---

_Unassigned @ibraheemdev by @MichaReiser on 2025-11-10 16:26_

---

_Closed by @carljm on 2025-11-26 16:50_

---
