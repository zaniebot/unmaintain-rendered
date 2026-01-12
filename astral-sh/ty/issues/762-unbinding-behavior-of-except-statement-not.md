```yaml
number: 762
title: "Unbinding behavior of `except` statement not modeled by ty"
type: issue
state: closed
author: UnboundVariable
labels: []
assignees: []
created_at: 2025-07-03T23:38:40Z
updated_at: 2025-07-04T01:17:34Z
url: https://github.com/astral-sh/ty/issues/762
synced_at: 2026-01-12T15:54:23Z
```

# Unbinding behavior of `except` statement not modeled by ty

---

_@UnboundVariable_

### Summary

When an `except` clause in a `try` statement has an `as` target, the runtime unbinds the target variable at the bottom of the `except` block. This is documented [here](https://docs.python.org/3/reference/compound_stmts.html#except-clause).

Ty doesn't appear to model this behavior currently, so it doesn't report `x` as being possibly unbound in the code sample below.

```python
def f():
    try:
        x = 0
        print(1 / 0)
    except ZeroDivisionError as x:
        pass

    print(x) # x is unbound here resulting in a runtime error
```



### Version

_No response_

---

_Comment by @carljm on 2025-07-04 01:17_

Thanks for the report!

We do model this behavior. It doesn't show up in your example because we disable the "possibly unbound" diagnostic by default (we've found too many cases where it leads to false positives in working code that relies on invariants not seen by the type checker). If we explicitly enable that diagnostic in the config, the error shows up in your example: https://play.ty.dev/ec6a8f29-9709-455d-ac1f-88f085f8e53a

Or if we modify your example slightly to use a different variable, so that it is definitely-unbound rather than possibly-unbound, we see that reported as well: https://play.ty.dev/587d7adc-40fc-4ca3-9577-3c6201773b4c

---

_Closed by @carljm on 2025-07-04 01:17_

---
