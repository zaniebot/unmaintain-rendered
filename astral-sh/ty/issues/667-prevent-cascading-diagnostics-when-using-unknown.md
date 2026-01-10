```yaml
number: 667
title: Prevent cascading diagnostics when using unknown type form
type: issue
state: open
author: sharkdp
labels:
  - bug
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-06-17T07:45:26Z
updated_at: 2025-11-18T16:10:33Z
url: https://github.com/astral-sh/ty/issues/667
synced_at: 2026-01-10T01:58:59Z
```

# Prevent cascading diagnostics when using unknown type form

---

_Issue opened by @sharkdp on 2025-06-17 07:45_

We currently report multiple diagnostics in situations like the following:

```py
# Name `UnknownTypeForm` used when not defined (unresolved-reference) [Ln 1, Col 4]
# Int literals are not allowed in this context in a type expression (invalid-type-form) [Ln 1, Col 20]
x: UnknownTypeForm[1]


# Name `UnknownTypeForm` used when not defined (unresolved-reference) [Ln 1, Col 4]
# Syntax error in forward annotation: Unexpected token at the end of an expression (invalid-syntax-in-forward-annotation) [Ln 1, Col 20]
x: UnknownTypeForm["some string"]
```

The first of these (`unresolved-reference`) is obviously fine, but I think we should suppress the other diagnostic. This can also cause false positives in unreachable code where legit type forms may be inferred as `Unknown`:

```py
assert False

from typing_extensions import Literal

def outer():
    # Syntax error in forward annotation: Unexpected token at the end of an expression (invalid-syntax-in-forward-annotation) [Ln 7, Col 26]
    def inner(p: Literal["missing value"]):
        pass
```

---

_Label `bug` added by @sharkdp on 2025-06-17 07:45_

---

_Label `diagnostics` added by @sharkdp on 2025-06-17 07:45_

---

_Label `help wanted` added by @sharkdp on 2025-06-17 07:45_

---

_Comment by @InSyncWithFoo on 2025-06-19 16:33_

A possible approach is to apply "name-based resolution" on some of the unresolved references:

```python
# `Literal` is unresolved, since it is not imported,
# but instead of treating it as `Unknown`,
# assume the user mean `typing[_extensions].Literal`.

#        vvvvvvv error: `Literal` is not resolved
def _(a: Literal['a b']):
#                ^^^^^ Fine
	reveal_type(a)        # Literal['a b']
	reveal_type(Literal)  # Error out, but reveal `typing[_extensions].Literal`
```

`reveal_type()` is already benefitting from this special treatment. I'm not sure how much of a good idea it is to extend to other symbols, though.

---

_Comment by @carljm on 2025-06-19 17:35_

I don't think we want to make assumptions based on the name.

It seems to me the likely fix here is just some kind of "visit and evaluate/store unknown" helper in type expression evaluation that doesn't emit diagnostics, so when we evaluate something as `Unknown`, we can just mark its subscript expression (and all sub-expressions) as also `Unknown` and move on, rather than really trying to evaluate them and possibly emitting diagnostics.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:10_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
