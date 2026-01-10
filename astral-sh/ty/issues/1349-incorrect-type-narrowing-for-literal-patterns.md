```yaml
number: 1349
title: Incorrect type narrowing for literal patterns
type: issue
state: closed
author: erictraut
labels:
  - bug
  - narrowing
  - runtime semantics
assignees: []
created_at: 2025-10-14T00:09:03Z
updated_at: 2025-10-16T07:50:34Z
url: https://github.com/astral-sh/ty/issues/1349
synced_at: 2026-01-10T02:06:25Z
```

# Incorrect type narrowing for literal patterns

---

_Issue opened by @erictraut on 2025-10-14 00:09_

### Summary

I just fixed a longstanding bug in pyright that ty appears to suffer from too. It's related to literal patterns in `case` statements. The runtime uses equality checks for literal pattern matching, but equality checks can succeed for non-overlapping types.

Consider the following:

```python
def func(x: int):
    match x:
        case 1.0:
            reveal_type(x) # ty incorrectly reveals Never here
            print("This code is reachable")

func(1)
```

### Version

_No response_

---

_Comment by @ericmarkmartin on 2025-10-14 03:43_

I think we got this right in visibility constraints but not narrowing @carljm ---in the former we [only do any narrowing if the subject type is single-valued](https://github.com/astral-sh/ruff/blob/5e08e5451df62398caf16034ae50621e46688641/crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs#L725-L730) but in the latter we [always narrow to subject type](https://github.com/astral-sh/ruff/blob/5e08e5451df62398caf16034ae50621e46688641/crates/ty_python_semantic/src/types/narrow.rs#L1002-L1003).

For singly-valued subjects we can still do some narrowing...

```py
def func(x: Literal["spam"]):
    match x:
        case 4:
            reveal_type(x)  # Never
```

but also if the subject is a union of singly-valued types

```py
def func(x: Literal["ham", "spam"] | None):
    match x:
        case "ham":
            reveal_type(x)  # Literal["ham"]
        case "spam":
            reveal_type(x)  # Literal["spam"]
        case "eggs":
            reveal_type(x)  # Never
```

or even a singly-valued type union'd to something else

```py
def func(x: Literal["ham", "spam"] | int):
    match x:
        case "ham":
            reveal_type(x)  # Literal["ham"] | int
        case "spam":
            reveal_type(x)  # Literal["spam"] | int
        case "eggs":
            reveal_type(x)  # int
```

Are there other cases you can do this?

Edit: We have this logic already implemented in [`evaluate_expr_eq`](https://github.com/astral-sh/ruff/blob/5e08e5451df62398caf16034ae50621e46688641/crates/ty_python_semantic/src/types/narrow.rs#L478)

---

_Comment by @ericmarkmartin on 2025-10-14 04:08_

@erictraut I tried to test these examples in the pyright playground but I think it hasn't picked up the fix you mentioned. Do you know if pyright does the above narrowing?

---

_Comment by @erictraut on 2025-10-14 05:26_

The pyright playground runs only published versions of pyright. The fix I mentioned will be in the next published version.

---

_Label `bug` added by @AlexWaygood on 2025-10-14 07:06_

---

_Label `narrowing` added by @AlexWaygood on 2025-10-14 07:06_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-10-14 07:06_

---

_Comment by @ericmarkmartin on 2025-10-14 12:55_

@AlexWaygood I'm happy to take this one

---

_Assigned to @ericmarkmartin by @AlexWaygood on 2025-10-14 13:03_

---

_Comment by @carljm on 2025-10-14 15:00_

> We have this logic already implemented in [`evaluate_expr_eq`](https://github.com/astral-sh/ruff/blob/5e08e5451df62398caf16034ae50621e46688641/crates/ty_python_semantic/src/types/narrow.rs#L478)

Yes, this should just be a matter of using that existing logic for match patterns also

---

_Closed by @sharkdp on 2025-10-16 07:50_

---
