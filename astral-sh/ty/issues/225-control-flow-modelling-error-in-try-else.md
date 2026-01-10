```yaml
number: 225
title: "Control flow modelling error in `try` ... `else`"
type: issue
state: open
author: sharkdp
labels:
  - control flow
  - runtime semantics
assignees: []
created_at: 2024-12-11T11:06:23Z
updated_at: 2025-11-25T12:20:34Z
url: https://github.com/astral-sh/ty/issues/225
synced_at: 2026-01-10T01:58:59Z
```

# Control flow modelling error in `try` ... `else`

---

_Issue opened by @sharkdp on 2024-12-11 11:06_

https://github.com/astral-sh/ty/issues/233 mentions a problem with `finally` suites, but it looks like we also don't model `try` … `else` control flow correctly in all cases.

We currently infer `Literal[1, 2, 3]` here, but it should be `Literal[1, 3]`?

```py
def may_raise() -> None: ...
def flag() -> bool: ...

x = 1

try:
    may_raise()
    x = 2
except KeyError:
    pass
else:
    x = 3

reveal_type(x)  # revealed: Literal[1, 2, 3]
```

---

_Comment by @AlexWaygood on 2024-12-11 11:53_

I spent a while thinking that this had something to do with the fact that we don't yet model the fact that simple statements like `x = 2` cannot ever raise exceptions (except for `KeyboardInterrupt`). But it doesn't actually have anything to do with that, I don't think. So let's use this example, which doesn't confuse the two issues in the same way ;)

```py
from typing import Literal

def may_raise() -> Literal[2]:
    return 2

x = 1
    
try:
    may_raise()
    x = may_raise()
except:
    pass
else:
    x = 3
    
reveal_type(x)  # revealed: Literal[1, 2, 3]
```

Like your example, we reveal `Literal[1, 2, 3]` here, but it's possible that we should ideally reveal `Literal[1, 3]`. This is because the possible control-flow paths are as follows:

1. One of the `may_raise()` calls fails with something other than `KeyError`. The scope immediately terminates; this isn't interesting to us and we don't need to consider it any further.
2. The first `try` `may_raise()` call fails with `KeyError`; we jump to the `except` branch before the `x` reassignment in the `try` statement. The `else` branch is not executed; `x` continues to have type `Literal[1]`.
3. The first `try` `may_raise()` call succeeds, but the second one fails with `KeyError`. Since the expression on the r.h.s. of the reassignment statement fails, `x` is never reassigned, so we jump to the `exception` block. The `else` branch is not executed; `x` continues to have type `Literal[1]`.
4. Both `try` `may_raise()` calls succeed; we skip to the `else` branch and the `except` branch is not executed. The `else` branch reassigns `x` again; the type of `x` is `Literal[3]` after we exit the entire `try`/`except` handler.

This indicates that to get the desired result here, we need to have some special handling for assignments when the assignment is the _final_ statement in a `try` suite. Either the assignment succeeds and we continue to the `else` suite, or the assignment does not succeed and we jump to the `except` suite.

### Concern about `KeyboardInterrupt`s

This feels like a massive edge case, but _is_ it possible to jump from a `try` suite to an `except` suite _even if_ all statements in the `try` actually succeed? What if something like this happens?

```py
from typing import Literal

def may_raise() -> Literal[2]:
    return 2

x = 1
    
try:
    may_raise()
    x = may_raise()
    # <-- !! KeyboardInterrupt is raised here because the user pressed CTRL+C !!
except:
    pass
else:
    x = 3
    
reveal_type(x)  # revealed: Literal[1, 2, 3]
```

I don't know if it's even possible for a `KeyboardInterrupt` to intercept control flow at that point, though. (I think I'd probably have to dig deep into CPython's internals to find out?) And even if it _is_ possible, I think it's probably not worth worrying about? It won't be intuitive for users to infer `Literal[1, 2, 3]` in these cases because of very obscure edge cases involving `KeyboardInterrupt` that would never happen in practice.

### Concern about assignment expressions

Consider this variation:

```py
from typing import Literal

def may_raise() -> Literal[2]:
    return 2

def may_also_raise(x: object) -> None: ...

x = 1
    
try:
    may_also_raise(x := may_raise())
except:
    pass
else:
    x = 3
    
reveal_type(x)  # revealed: Literal[1, 2, 3]
```

Here we _should_ infer `Literal[1, 2, 3]`, because it's very possible that the reassignment to `x` succeeds in the `try` suite even though the reassignment's enclosing statement fails: the inner expression in the `try`-block final statement (which reassigns `x`) is executed before the outer expression in the `try`-block final statement. So it's not enough to merely consider whether an assignment takes place in the final statement of a `try` block: assignment expressions require special handling.

---

_Comment by @AlexWaygood on 2024-12-12 10:42_

In fact, even for assignments that are created via statements rather than assignment expressions, simply looking at whether the assignment occurs in "the last statement" of the `try` suite would be too simplistic. In this snippet, the reassignment to `x` occurs in "the last statement" of the `try` suite (which is a `StmtIf` node), but it's clearly possible for us to jump to the `except` suite after `x` has been successfully reassigned to `2`, since other statements occur after the reassignment that are also substatements of "the last statement" of the `try` suite:

```py
def may_raise(): ...

x = 1

try:
    may_raise()
    if True:
        x = 2
        may_raise()
except:
    pass
else:
    x = 3
```

We would have to recursively examine whether the assignment takes place in the last statement of the last statement of the last statement of [etc. for however many substatements there are] the `try` suite.

---

_Comment by @carljm on 2024-12-14 01:48_

Great analysis! I don't think we'd have to do anything recursive, though; it should be sufficient to just check if the assignment statement is the last statement in the body of the current try block. 

---

_Renamed from "[red-knot] Control flow modelling error in `try` ... `else`" to "Control flow modelling error in `try` ... `else`" by @MichaReiser on 2025-05-07 15:27_

---

_Label `control flow` added by @AlexWaygood on 2025-05-10 21:38_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:48_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:00_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @sharkdp on 2025-11-25 12:20_

Another case that came up in a real world project (modified here to make it a standalone example):
```py
def _(x: int | str):
    if isinstance(x, str):
        try:
            x = int(x)
        except ValueError:
            reveal_type(x)  # can only be `str`, but we infer `str | int`
```
(https://play.ty.dev/19d16684-ccb8-41c3-942f-60ce61e090ee)

The only statement that can raise the exception is `x = int(x)` (either during the call, or theoretically during the assignment), and if that *does* raise, the type of `x` should still be `str`. However, ty infers `str | int`, because we do not recognize that nothing after `x = int(x)` could possibly raise the exception.

Pyrefly and pyright infer `str` here. mypy also infers `int | str`.

---
