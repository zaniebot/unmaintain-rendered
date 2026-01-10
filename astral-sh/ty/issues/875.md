```yaml
number: 875
title: names bound at some point in a class scope, but not at point of access, fall back to global scope
type: issue
state: closed
author: carljm
labels:
  - bug
  - help wanted
  - runtime semantics
assignees: []
created_at: 2025-07-22T20:54:26Z
updated_at: 2025-08-05T03:21:29Z
url: https://github.com/astral-sh/ty/issues/875
synced_at: 2026-01-10T02:06:24Z
```

# names bound at some point in a class scope, but not at point of access, fall back to global scope

---

_Issue opened by @carljm on 2025-07-22 20:54_

Python's scoping semantics in class scopes are really quite odd. There's a subtle difference at runtime between these two examples:

```py
x: str

def f(x: int):
    class C:
        reveal_type(x)  # revealed: int

def g(x: int):
    class C:
        reveal_type(x)  # revealed: str
        x = None
```

In both examples, `x` has no value assigned to it in the scope of class `C` at the point where it is referenced (in the `reveal_type`).

In the case of `f` where `x` is never bound in the scope of `class C`, at runtime a `LOAD_DICT_OR_DEREF` opcode is used, which will fall back to the `x` in the scope of `f`. But if `x` _is_ bound at any point in the scope of `C` (as occurs in `g`), then a `LOAD_NAME` is used instead, which falls back only to the global scope, bypassing any enclosing function scopes.

Currently we don't model this difference correctly, so we wrongly infer `int` in both cases above. It should be a fairly simple change to `TypeInferenceBuilder::infer_place_load` to fix this (I think).

---

_Label `bug` added by @carljm on 2025-07-22 20:54_

---

_Label `help wanted` added by @carljm on 2025-07-22 20:54_

---

_Label `runtime semantics` added by @carljm on 2025-07-22 20:54_

---

_Comment by @erictraut on 2025-07-22 20:59_

Yeah, mypy doesn't model this correctly either.

Probably not a high priority.

It took quite a bit of work to model this correctly in pyright. Looks like pyrefly models it too.

---

_Comment by @carljm on 2025-07-22 21:01_

Yeah, both mypy and pyrefly get it wrong.

I _think_ it should be pretty easy for us to get it right? But if I'm wrong, I agree it's not a high priority to spend a lot of time on.

---

_Closed by @carljm on 2025-08-05 03:21_

---
