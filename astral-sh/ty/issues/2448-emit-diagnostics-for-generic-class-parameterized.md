```yaml
number: 2448
title: Emit diagnostics for generic class parameterized with a single-element tuple of types if the generic class accepts only a single type argument
type: issue
state: open
author: hyperkai
labels:
  - needs-info
assignees: []
created_at: 2026-01-11T17:06:23Z
updated_at: 2026-01-12T13:18:28Z
url: https://github.com/astral-sh/ty/issues/2448
synced_at: 2026-01-12T14:02:46Z
```

# Emit diagnostics for generic class parameterized with a single-element tuple of types if the generic class accepts only a single type argument

---

_Issue opened by @hyperkai on 2026-01-11 17:06_

### Summary

*Memo:
- ty check
- ty 0.0.7
- Python 3.14.0

A generic type argument accepts a tuple as shown below:

```python
       # ↓↓↓↓↓↓
v1: list[(int,)]
v1 = [0, 1, 2]       # No error
v1 = ['A', 'B', 'C'] # Error

       # ↓↓↓↓↓↓
v2: list[(str,)]
v2 = [0, 1, 2]       # Error
v2 = ['A', 'B', 'C'] # No error
```

```python
        # ↓↓↓↓↓↓↓↓↓↓
v1: tuple[(int, ...)]
v1 = (0, 1, 2)       # No error
v1 = ('A', 'B', 'C') # Error

        # ↓↓↓↓↓↓↓↓↓↓
v2: tuple[(str, ...)]
v2 = (0, 1, 2)       # Error
v2 = ('A', 'B', 'C') # No error
```

### Version

_No response_

---

_Comment by @AlexWaygood on 2026-01-11 17:36_

`tuple[(int, ...)]` and `tuple[(str, ...)]` seem fine to me, since they have almost exactly the same AST as `tuple[int, ...]` and `tuple[str, ...]` (respectively). I agree we should emit diagnostics for `list[(int,)]` and `list[(str,)]`, though.

---

_Renamed from "A generic type argument accepts a tuple" to "Emit diagnostics for generic class parameterized with a single-element tuple of types if the generic class accepts only a single type argument" by @AlexWaygood on 2026-01-11 17:37_

---

_Label `diagnostics` added by @dhruvmanila on 2026-01-12 05:57_

---

_Comment by @dhruvmanila on 2026-01-12 06:13_

> I agree we should emit diagnostics for `list[(int,)]` and `list[(str,)]`, though.

I don't think we should emit a diagnostic here as this is a valid specialization because a tuple of types gets matched against individual type parameters in the generic class in the same way as `list[int, str]` (which is a tuple in the AST) or `list[(int, str)]` does. None of the other type checkers emit a diagnostic either.

@hyperkai Can you clarify what issue are you facing? Are you expecting that ty should raise an error where you've mentioned "No error" as a comment? Or is it something else?

---

_Label `diagnostics` removed by @dhruvmanila on 2026-01-12 06:13_

---

_Label `needs-info` added by @dhruvmanila on 2026-01-12 06:14_

---

_Comment by @AlexWaygood on 2026-01-12 12:43_

The same issue was reported to mypy and the maintainers there made a similar argument to @dhruvmanila that `list[(int,)]` should be considered valid: https://github.com/python/mypy/issues/20563. It does at the very least seem "highly distasteful" to me, but I can see the consistency argument for allowing it 😆

Pyrefly also got the same bug report, but the pyrefly maintainers haven't made a decision yet: https://github.com/facebook/pyrefly/issues/2063. Pyright, too -- https://github.com/microsoft/pyright/issues/11224 -- though the rationale given there for closing that issue doesn't seem to me to be accurate (`list[(int,)]` is not "the same" as `list[int]` -- they have very different ASTs, even if type checkers can reasonably choose to treat them differently.)

---

_Comment by @MichaReiser on 2026-01-12 13:14_

>  though the rationale given there for closing that issue doesn't seem to me to be accurate (list[(int,)] is not "the same" as list[int] 

I believe the intended argument is that `list[(int,)]` and `list[int,]` parse to the same AST:

```pycon
>>> ast.dump(ast.parse('list[(int,)]'))
"Module(body=[Expr(value=Subscript(value=Name(id='list', ctx=Load()), slice=Tuple(elts=[Name(id='int', ctx=Load())], ctx=Load()), ctx=Load()))])"
>>> ast.dump(ast.parse('list[int,]'))
"Module(body=[Expr(value=Subscript(value=Name(id='list', ctx=Load()), slice=Tuple(elts=[Name(id='int', ctx=Load())], ctx=Load()), ctx=Load()))])"
```

---

_Comment by @AlexWaygood on 2026-01-12 13:16_

> I believe the intended argument is that `list[(int,)]` and `list[int,]` parse to the same AST:

I understand that, but the relevant question is not whether we should treat `list[(int,)]` the same as `list[int,]`. I agree we should treat those two the same, and I said as much in https://github.com/astral-sh/ty/issues/2448#issuecomment-3735144048. The relevant question is whether we should treat `list[(int,)]` or `list[int,]` the same as `list[int]`.

---
