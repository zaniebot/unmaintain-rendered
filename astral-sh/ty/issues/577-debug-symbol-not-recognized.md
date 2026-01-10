```yaml
number: 577
title: __debug__ symbol not recognized
type: issue
state: closed
author: RianKojaASQ
labels:
  - bug
  - help wanted
  - runtime semantics
assignees: []
created_at: 2025-06-03T17:14:50Z
updated_at: 2025-06-10T06:49:00Z
url: https://github.com/astral-sh/ty/issues/577
synced_at: 2026-01-10T02:34:10Z
```

# __debug__ symbol not recognized

---

_Issue opened by @RianKojaASQ on 2025-06-03 17:14_

### Summary

Just got this warning on vscode:

`Name '__debug__' used when not definedty[unresolved-reference](https://ty.dev/rules#unresolved-reference)`


Recalling the docs:  https://docs.python.org/3/library/constants.html

**`__debug__`**
This constant is true if Python was not started with an [-O](https://docs.python.org/3/using/cmdline.html#cmdoption-O) option. See also the [assert](https://docs.python.org/3/reference/simple_stmts.html#assert) statement.




### Version

0.0.1a8

---

_Label `bug` added by @AlexWaygood on 2025-06-03 17:16_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-06-03 17:16_

---

_Comment by @AlexWaygood on 2025-06-03 17:51_

This should be easy to fix -- it's an implicit global symbol that doesn't appear in typeshed's stubs, similar to `__builtins__`. We need to add a branch here like the one for `__builtins__`: https://github.com/astral-sh/ruff/blob/e8ea40012afda872ef9bdf496a8b888ba80b533b/crates/ty_python_semantic/src/symbol.rs#L327-L328

---

_Label `help wanted` added by @AlexWaygood on 2025-06-03 17:51_

---

_Comment by @jelle-openai on 2025-06-03 17:56_

Note `__debug__` is a little more special than that:

```
>>> __debug__ = 1
  File "<stdin>", line 1
SyntaxError: cannot assign to __debug__
```

Up to you how important it is to replicate that special treatment in ty, though.

---

_Comment by @AlexWaygood on 2025-06-03 17:57_

We already implement the syntax error! https://play.ty.dev/1e89682b-1ef8-464b-b7a6-7814cdd37237

---

_Comment by @suneettipirneni on 2025-06-04 02:24_

I'd like to work on this issue if it's not already in progress.

---

_Assigned to @suneettipirneni by @carljm on 2025-06-04 04:48_

---

_Comment by @carljm on 2025-06-04 04:50_

@suneettipirneni It's yours! Feel free to ask here or [in our Discord server](https://discord.com/invite/astral-sh) if you have questions about the implementation.

---

_Closed by @sharkdp on 2025-06-10 06:49_

---
