```yaml
number: 365
title: "panic: If we have at least one binding, the scope-start should not be definitely visible"
type: issue
state: closed
author: jelle-openai
labels:
  - bug
  - fatal
  - control flow
assignees: []
created_at: 2025-05-13T18:29:08Z
updated_at: 2025-06-17T07:24:30Z
url: https://github.com/astral-sh/ty/issues/365
synced_at: 2026-01-10T02:08:20Z
```

# panic: If we have at least one binding, the scope-start should not be definitely visible

---

_Issue opened by @jelle-openai on 2025-05-13 18:29_

### Summary

```
error[panic]: Panicked at crates/ty_python_semantic/src/symbol.rs:824:17 when checking `<snip>.py`: `internal error: entered unreachable code: If we have at least one binding, the scope-start should not be definitely visible`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "."]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: symbol_by_id(Id(1c51a9b))
             at crates/ty_python_semantic/src/symbol.rs:576
   1: infer_expression_types(Id(1c6a4a5))
             at crates/ty_python_semantic/src/types/infer.rs:222
   2: infer_scope_types(Id(1c257c7))
             at crates/ty_python_semantic/src/types/infer.rs:121
   3: check_types(Id(4a7b))
             at crates/ty_python_semantic/src/types.rs:84
```

As before, can try to minimize if this isn't enough to tell the source of the crash.

### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Comment by @AlexWaygood on 2025-05-13 18:30_

That *is* a fun one! Yes please, a minimal repro would be great here.

---

_Label `fatal` added by @AlexWaygood on 2025-05-13 18:30_

---

_Label `control flow` added by @AlexWaygood on 2025-05-13 18:30_

---

_Label `bug` added by @AlexWaygood on 2025-05-13 18:30_

---

_Comment by @carljm on 2025-05-13 18:31_

Will probably need an example to fix this -- but I think there is also a mypy-primer project where we see it, so we should be able to track it down there and fix it, and then see if that fixes your case as well. Thanks for the report!

---

_Label `bug` removed by @carljm on 2025-05-13 18:31_

---

_Label `control flow` removed by @carljm on 2025-05-13 18:31_

---

_Label `bug` added by @AlexWaygood on 2025-05-13 18:32_

---

_Label `control flow` added by @AlexWaygood on 2025-05-13 18:32_

---

_Comment by @jelle-openai on 2025-05-13 18:40_

Here you go:

```python
def f():
    pass


def g(a):
    if not c.get("t"):
        return


if __name__ == "__main__":
    while True:
        b = f()
        if b == -1:
            break

        c = h()
        if not c:
            break
```

So the global `c` is being used inside function `g()`, but it's only defined conditionally in the `if __name__` block.

---

_Comment by @sharkdp on 2025-05-15 18:41_

Thank you very much. I further narrowed it down to
```py
def g():
    c

while True:
    if flag1:
        break

    c = 1

    break
```

Interestingly, we do not panic for the following, seemingly equivalent `if` construct:
```py
if True:
    if flag1:
        pass
    else:
        c = 1
```

So it looks like this has something to do with `while` loops or `break` statements.

---

_Assigned to @sharkdp by @sharkdp on 2025-05-15 18:41_

---

_Comment by @sharkdp on 2025-05-15 20:20_

Even simpler:
```py
while True:
    if flag1:
        break
    else:
        c = 1
        break

c
```

Interestingly, if we move the `break` out of the conditional branches… 
```py
while True:
    if flag1:
        pass
    else:
        c = 1

    break

c
```
… it does not panic

---

_Comment by @sharkdp on 2025-05-16 07:47_

I think I know what the problem is. When we save break snapshots, we fail to update them with visibility constraints. So for the first `break` here …
```py
while True:
    if flag1:
        break
    else:
        c = 1
        break

c
```
… we fail to record *any* visibility constraints (for the implicit `c = <unbound>` binding at the start of scope), because they are only added at the end of control flow branches. When we later merge in the break states after the loop, we are therefore left with a `c = <unbound>` definition which is still `AlwaysTrue`, leading to that panic

This can also lead to wrong type inference:
```py
c = 1

while True:
    if False:
        c = 2
        break
    break

reveal_type(c)  # should be `1`, but we reveal `1, 2`
```

---

_Comment by @carljm on 2025-05-22 03:18_

We track both visibility constraints (added at the end of a branch) and reachability constraints (added at the start of the branch). If we updated the reachability constraint, and visibility constraints for all pre-existing bindings, at the start of the branch, and then created new bindings in the branch with initial visibility constraints matching reachability constraints at the point of the binding, would that work and potentially solve this problem? Or am I missing another reason why have to add branch visibility constraints at the end of the branch?

---

_Comment by @sharkdp on 2025-05-22 06:30_

(I put work here a bit on hold since https://github.com/astral-sh/ruff/pull/18041 was also making major changes to the semantic index builder, and there were other things to do)

> If we updated the reachability constraint, and visibility constraints for all pre-existing bindings, at the start of the branch, and then created new bindings in the branch with initial visibility constraints matching reachability constraints at the point of the binding, would that work and potentially solve this problem?

This sounds like it can work, yes! And it might actually be a major simplification of the code.

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:00_

---

_Closed by @sharkdp on 2025-06-17 07:24_

---
