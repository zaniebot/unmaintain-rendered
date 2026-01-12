```yaml
number: 369
title: Non-deterministic output on hydra-zen
type: issue
state: closed
author: sharkdp
labels:
  - bug
assignees: []
created_at: 2025-05-13T20:34:07Z
updated_at: 2025-05-15T02:48:34Z
url: https://github.com/astral-sh/ty/issues/369
synced_at: 2026-01-12T15:54:23Z
```

# Non-deterministic output on hydra-zen

---

_@sharkdp_

## How to reproduce

Set up the project the mypy_primer way:
```bash
git clone https://github.com/mit-ll-responsible-ai/hydra-zen
cd hydra-zen
uv venv
uv pip install pydantic beartype hydra-core
source .venv/bin/activate
```

Then run the following command repeatedly:
```
ty check src tests/annotations
```

Leads to either 541, 543, or 544 diagnostics.

When `TY_MAX_PARALLELISM` is set to `1`, ty consistently reports 544 diagnostics.

---

_Label `bug` added by @sharkdp on 2025-05-13 20:34_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-13 20:56_

---

_Comment by @MichaReiser on 2025-05-14 05:47_

This is pure speculation. I wonder if this happens because of different runs use different cycle heads (we enter cycles with different entry function depending on whether the project is checked in parallel or not). May in combination with generics?

I'm seeing something similar in https://github.com/salsa-rs/salsa/issues/831

---

_Comment by @sharkdp on 2025-05-14 08:21_

Ouff... this took me a long time to narrow down. It seems to require three different files:

`mod1.py`

```py
class A[T]:
    pass


class B[T]:
    pass
```

`mod2.py`

```py
from mod1 import A, B

def f[T](f: bool) -> A[T] | B[T]:
    raise NotImplementedError
```


`main.py`

```py
from typing import TypeAlias, assert_type

from mod2 import f
from mod1 import A, B


SomeTodoType: TypeAlias = int


def _(flag: bool):
    assert_type(
        f(flag),
        A[SomeTodoType] | B[SomeTodoType],
    )
```


Then I can run the following in the folder with these three files:

```
> while true; do ty check --output-format concise 2>&1 | rg '(Found|checks)'; done
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
All checks passed!
Found 1 diagnostic
Found 1 diagnostic
All checks passed!
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
All checks passed!
All checks passed!
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
All checks passed!
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
Found 1 diagnostic
```


There are 6 permutations in which you can check these three files. When running single-threaded, one of them leads to a diagnostic being emitted, the other five run successfully:
```
▶ TY_MAX_PARALLELISM=1 ty check main.py mod1.py mod2.py
All checks passed!

▶ TY_MAX_PARALLELISM=1 ty check main.py mod2.py mod1.py
error[type-assertion-failure]: Argument does not have asserted type `A[@Todo] | B[@Todo]`
  --> main.py:11:5
   |
10 |   def _(flag: bool):
11 | /     assert_type(
12 | |         f(flag),
   | |         ------- Inferred type of argument is `A[Unknown] | B[Unknown]`
13 | |         A[SomeTodoType] | B[SomeTodoType],
14 | |     )
   | |_____^
   |
info: `A[@Todo] | B[@Todo]` and `A[Unknown] | B[Unknown]` are not equivalent types
info: rule `type-assertion-failure` is enabled by default

Found 1 diagnostic

▶ TY_MAX_PARALLELISM=1 ty check mod1.py main.py mod2.py
All checks passed!

▶ TY_MAX_PARALLELISM=1 ty check mod1.py mod2.py main.py
All checks passed!

▶ TY_MAX_PARALLELISM=1 ty check mod2.py main.py mod1.py
All checks passed!

▶ TY_MAX_PARALLELISM=1 ty check mod2.py mod1.py main.py
All checks passed!
```

Interestingly, we infer `A[Unknown] | B[Unknown]` for `f(flag)` in all six cases, but only one of them leads to the diagnostic.

> I wonder if this happens because of different runs use different cycle heads (we enter cycles with different entry function depending on whether the project is checked in parallel or not). May in combination with generics?

I don't think so. There's nothing self-referential here and I don't see any fixpoint iteration happening in the traces.

But I have some other leads (e.g. type normalization, representation of `@Todo` types) that I want to follow...

---

_Comment by @AlexWaygood on 2025-05-15 01:06_

From conversations with @sharkdp on Discord, I have managed to find two bugs to do with type normalisation and unions. Once both of these bugs are fixed, the non-deterministic output on hydra-zen no longer occurs.

Bug number 1: equivalent unions that include `Any`, `Unknown` and/or `Todo` are simplified differently depending on their order in the source code:

```py
from typing import Any
from ty_extensions import Unknown

def f(x: Any | Unknown, y: Unknown | Any):
    reveal_type(x)  # Any
    reveal_type(y)  # Unknown
```

Bug number 2: equivalent unions that include equivalent-but-not-equal generic instance types are not understood as equivalent:

```py
from ty_extensions import is_equivalent_to

class Foo[T]: ...

reveal_type(is_equivalent_to(Foo[int | str] | str, str | Foo[str | int]))  # revealed `Literal[False]`, but it should reveal `Literal[True]`
```

Once both of these bugs are fixed, I no longer see any non-deterministic output on hydra-zen locally.

---

_Closed by @AlexWaygood on 2025-05-15 02:48_

---
