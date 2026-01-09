---
number: 17224
title: "add rule to catch `any` and `callable` in typing context"
type: issue
state: open
author: vpoverennov
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-04-05T20:22:47Z
updated_at: 2025-04-07T12:18:47Z
url: https://github.com/astral-sh/ruff/issues/17224
synced_at: 2026-01-07T13:12:16-06:00
---

# add rule to catch `any` and `callable` in typing context

---

_Issue opened by @vpoverennov on 2025-04-05 20:22_

### Summary

Here's the code
```
def f(x: any):
    pass
```
This sometimes happens when switching between typescript and python, or you just don't notice the typo until later because `any` is a builtin python function (type checkers are slower to run than linters)


<details>
<summary>Type Checker results</summary>

Code:
```
def f1(x: any):
    pass
def f2(x: callable):
    pass
def f3(x: dict):
    pass
def f4(x: frozenset):
    pass
def f5(x: list):
    pass
def f6(x: set):
    pass
def f7(x: tuple):
    pass
def f8(x: type):
    pass
```

mypy
```
x.py:1: error: Function "builtins.any" is not valid as a type  [valid-type]
x.py:1: note: Perhaps you meant "typing.Any" instead of "any"?
x.py:3: error: Function "builtins.callable" is not valid as a type  [valid-type]
x.py:3: note: Perhaps you meant "typing.Callable" instead of "callable"?
Found 2 errors in 1 file (checked 1 source file)
```
pyright
```
x.py:1:11 - error: Expected class but received "(iterable: Iterable[object], /) -> bool" (reportGeneralTypeIssues)
x.py:3:11 - error: Expected class but received "(obj: object, /) -> TypeIs[(...) -> object]" (reportGeneralTypeIssues)
```
red knot
```
Variable of type `Literal[any]` is not allowed in a type expression (lint:invalid-type-form) [Ln 1, Col 11]
Variable of type `Literal[callable]` is not allowed in a type expression (lint:invalid-type-form) [Ln 3, Col 11]
```
</details>

But it would be nice to have it caught AND autofixed by ruff

Here are all name collisions between builtins and `typing` (lowercased)
```
['any', 'callable', 'dict', 'frozenset', 'list', 'set', 'tuple', 'type']
```
Only those stick out - `any` and `callable`, the rest are already valid as the type hints
The rule would be to flag uses of `any` and `callable` as types and convert them to `typing.Any` and `typing.Callable` respectively.

Related rules:
[non-pep585-annotation (UP006)](https://docs.astral.sh/ruff/rules/non-pep585-annotation/#non-pep585-annotation-up006)
[any-type (ANN401)](https://docs.astral.sh/ruff/rules/any-type/#any-type-ann401)

Would be curious to get your thoughts for such a rule, thank you for making the awesome tool

---

_Label `rule` added by @ntBre on 2025-04-06 00:29_

---

_Label `needs-decision` added by @MichaReiser on 2025-04-07 08:14_

---

_Comment by @MichaReiser on 2025-04-07 08:15_

I think such a rule makes sense but I'm not sure I want to implement it in Ruff, considering that we already support it in Red Knot.

---

_Comment by @vpoverennov on 2025-04-07 12:11_

@MichaReiser does (would) red-knot support auto-fixing the code, or would it only be checking it? To me the lack of autofix would be an argument for having the rule in ruff. One use-case for the autofix is to adopt a type checker in a codebase that previously didn't have any and already has code with typos like this like this in place (big volume). And in general having autofix for the problem is more helpful than having the linter/type checker tell you to fix it manually.
If red-knot does have the option to autofix this then probably doesn't make sense to implement in ruff itself

Sounds like you don't mind having the rule, if you don't want to invest resources into implementing it I can give it a shot myself

---

_Comment by @MichaReiser on 2025-04-07 12:18_

There's no auto fixing in the initial release but we should add support for auto fixes later

---

_Referenced in [aws-powertools/powertools-lambda-python#6466](../../aws-powertools/powertools-lambda-python/issues/6466.md) on 2025-04-15 13:09_

---
