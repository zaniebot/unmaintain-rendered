```yaml
number: 769
title: "`UnboundLocalError` not detected when variable not declared as `global` or `nonlocal` is `del`'d"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-07-05T17:14:34Z
updated_at: 2025-07-18T21:58:34Z
url: https://github.com/astral-sh/ty/issues/769
synced_at: 2026-01-10T02:07:36Z
```

# `UnboundLocalError` not detected when variable not declared as `global` or `nonlocal` is `del`'d

---

_Issue opened by @AlexWaygood on 2025-07-05 17:14_

### Summary

Ty doesn't emit any error on this code, but it should:

```py
X = 42

def f():
    print(X)

    if False:
        del X
```

At runtime, calling `f()` will always fail:

```pycon
>>> X = 42
>>> def f():
...     print(X)
...     if False:
...         del X
...         
>>> f()
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    f()
    ~^^
  File "<python-input-2>", line 2, in f
    print(X)
          ^
UnboundLocalError: cannot access local variable 'X' where it is not associated with a value
```

The fact that there is a `del` statement affecting the `X` variable later on in the function means that `X` must be a local variable for the duration of the entire function scope. I.e., the `print()` call on the first line of the function that attempts to load the symbol `X` cannot reach the definition of `X` in the global scope. `X` can _only_ ever be a local variable inside this function due to the later `del` statement, even though that `del` statement is unreachable.

Practically speaking, we need to edit this check so that it also checks whether there were any _deletions_ in this scope as well as any _bindings_ in this scope, and returns `Place::Unbound` if so: https://github.com/astral-sh/ruff/blob/44f2f77748f5ed36b78243075c849a091adfaaf8/crates/ty_python_semantic/src/types/infer.rs#L5708-L5719

@oconnor663, I wonder if you might be interested in this? It's sort-of adjacent to your work on the `nonlocal` statement :-)

### Version

_No response_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-07-05 17:14_

---

_Renamed from "`UnboundLocalError` not detected when variable not declared as `global` or `nonlocal` is del'd" to "`UnboundLocalError` not detected when variable not declared as `global` or `nonlocal` is `del`'d" by @AlexWaygood on 2025-07-05 17:14_

---

_Label `bug` added by @AlexWaygood on 2025-07-05 17:14_

---

_Assigned to @oconnor663 by @oconnor663 on 2025-07-07 14:23_

---

_Comment by @oconnor663 on 2025-07-07 14:23_

Nice, I'll take it!

---

_Comment by @oconnor663 on 2025-07-16 16:09_

There's a [related TODO](https://github.com/astral-sh/ruff/blob/291699b375f694c659fe82bdd68b149bde2494ee/crates/ty_python_semantic/resources/mdtest/del.md#L45-L47) to lint on `del x` when there is no `x` in the local scope:

```py
def delete():
    # TODO: this results in `UnboundLocalError`; we should emit `unresolved-reference`
    del x
```

Maybe once "`del [name]` requires `[name]` in the local scope" is enforced, then we won't need to implement "`del [name]` causes all uses of `[name]` to resolve to the local binding" as a separate rule, because the only way to make the `del` legal will be to make a local binding, and that has the same effect?

---

_Comment by @AlexWaygood on 2025-07-16 16:35_

> Maybe once "`del [name]` requires `[name]` in the local scope" is enforced, then we won't need to implement "`del [name]` causes all uses of `[name]` to resolve to the local binding" as a separate rule, because the only way to make the `del` legal will be to make a local binding, and that has the same effect?

I think the better strategy would be the opposite: if we implement "`del [name]` causes all uses of `[name]` to resolve to the local binding", we shouldn't need a separate implementation for `"del [name] requires [name] in the local scope"` — it should naturally fall out of the semantics of the first rule. And the first rule is the more important one, because we won't (or, shouldn't, at least!) emit an `unresolved-reference` diagnostic for a `del` statement referring to an unresolved reference if the `del` statement is in a branch that we determine to be unreachable at type-inference time — but even if the `del` statement is unreachable, it _still_ impacts the resolution of that name for all other loads in that scope

---

_Comment by @MichaReiser on 2025-07-16 16:36_

@AlexWaygood this must have been painful to type out on your phone with all the backticks ;)

---

_Comment by @AlexWaygood on 2025-07-16 16:37_

Shush

---

_Closed by @carljm on 2025-07-18 21:58_

---
