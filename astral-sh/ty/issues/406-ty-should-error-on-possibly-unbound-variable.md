```yaml
number: 406
title: "ty should error on possibly unbound variable usage after exception, but allow guarded access via `locals()` check"
type: issue
state: open
author: diego-pm
labels:
  - wish
  - control flow
assignees: []
created_at: 2025-05-15T10:28:11Z
updated_at: 2025-11-18T16:10:28Z
url: https://github.com/astral-sh/ty/issues/406
synced_at: 2026-01-10T01:58:59Z
```

# ty should error on possibly unbound variable usage after exception, but allow guarded access via `locals()` check

---

_Issue opened by @diego-pm on 2025-05-15 10:28_

ty currently does not detect possible unbound variables in a try-except block, as show in the following example:

```python
from typing import reveal_type

def could_raise() -> None: ...
def foo() -> str: ...  # ty: ignore[invalid-return-type]

try:
    could_raise()
    a = foo()
    could_raise()
except:
    reveal_type(a)  # str
    print(a)  # should error because 'a' is possibly unbound
    if "a" in locals():
        reveal_type(a)  # str
        print(a)  # should pass
```

Also, the guards `"a" in locals()` or `"a" in globals()` should allow accessing the variable wihout error.

---

_Comment by @sharkdp on 2025-05-15 11:25_

Thank you for your report.

> ty currently does not detect possible unbound variables in a try-except block, as show in the following example:

We do detect them, but the [`possibly-unresolved-reference`](https://github.com/astral-sh/ty/blob/main/docs/reference/rules.md#possibly-unresolved-reference) rule is disabled by default. When you enable it, you'll see it being emitted for your example: https://play.ty.dev/0c9f4801-c646-45b4-98e5-fc76d458fb89 (notice the setting in the `ty.json` tab).

> Also, the guards `"a" in locals()` or `"a" in globals()` should allow accessing the variable wihout error.

This is something that we don't support. It might be difficult to add, but I think it makes sense to discuss this here as a feature request.

It doesn't look like pyright supports this? And mypy doesn't emit a possibly-unbound diagnostic at all.

---

_Label `wish` added by @sharkdp on 2025-05-15 11:25_

---

_Label `control flow` added by @sharkdp on 2025-05-15 11:25_

---

_Comment by @diego-pm on 2025-05-15 11:41_

> > ty currently does not detect possible unbound variables in a try-except block, as show in the following example:
>
> We do detect them, but the [`possibly-unresolved-reference`](https://github.com/astral-sh/ty/blob/main/docs/reference/rules.md#possibly-unresolved-reference) rule is disabled by default. When you enable it, you'll see it being emitted for your example: [play.ty.dev/0c9f4801-c646-45b4-98e5-fc76d458fb89](https://play.ty.dev/0c9f4801-c646-45b4-98e5-fc76d458fb89) (notice the setting in the `ty.json` tab).

Sorry, did not realize that the related rule was disabled by default.

> > Also, the guards `"a" in locals()` or `"a" in globals()` should allow accessing the variable wihout error.
> 
> This is something that we don't support. It might be difficult to add, but I think it makes sense to discuss this here as a feature request.

That would be a great feature imao. Otherwise, I cannot think of another way for accessing the variable in this scenario, in the except block, without triggering a type error or having to include a type ignore comment.

> It doesn't look like pyright supports this? And mypy doesn't emit a possibly-unbound diagnostic at all.

pyright does not support this guard afaik.



---

_Comment by @diego-pm on 2025-05-15 12:06_

If the feature is too complex to implement, a partial inference that infers the type of `a` as `str | Unkown` would still be useful, right?

---

_Comment by @AlexWaygood on 2025-05-15 12:24_

> It doesn't look like pyright supports this? And mypy doesn't emit a possibly-unbound diagnostic at all.

FWIW, mypy issues the same possibly-unbound diagnostics as pyright if you pass `--enable-error-code=possibly-undefined` on the CLI — its version of this rule, like ty's, is also disabled by default: https://mypy-play.net/?mypy=latest&python=3.12&enable-error-code=possibly-undefined&gist=8518efbb1187bbdd528bf84b23e959f7

---

_Comment by @sharkdp on 2025-05-15 12:26_

> If the feature is too complex to implement, a partial inference that infers the type of `a` as `str | Unkown` would still be useful, right?

We track boundness of variables separate from the type. In your example, or in this simpler example below, we can tell for sure that the type is `str`. But the variable coulds still be unbound. What's not possible is that the variable is bound, but it has some unknown type. That can not happen
```py
def _(flag: bool, a_string: str):
    if flag:
        a = a_string

    reveal_type(a)  # type is definitely str, but `a` could be possibly unbound
```

---

_Comment by @carljm on 2025-05-15 15:07_

I think implementing support for the `"a" in locals()` check is feasible, but low priority relative to many other things we need to add, especially considering that existing type checkers do not support it either.

The typical recommendation is to make `try` blocks as narrowly scoped as possible, and the example case here is one reason for that. If separate try blocks were used for the two possibly-failing calls, the need to detect whether "a" has been bound or not would not arise.

But in general, the fact that it is not feasible for a static analyzer to understand all possible patterns for ensuring an invariant that some code path has been taken, is the reason why the `possibly-unbound-reference` diagnostic is not enabled by default. It ends up requiring that some working code patterns be rewritten to accommodate the capabilities of the static analysis.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
