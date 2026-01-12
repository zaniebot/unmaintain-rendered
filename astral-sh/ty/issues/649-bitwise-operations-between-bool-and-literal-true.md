```yaml
number: 649
title: "Bitwise operations between `bool` and `Literal[True]` are inferred as `int` instead of `bool`"
type: issue
state: closed
author: pt1243
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-06-13T14:15:54Z
updated_at: 2025-06-17T17:02:41Z
url: https://github.com/astral-sh/ty/issues/649
synced_at: 2026-01-12T15:54:23Z
```

# Bitwise operations between `bool` and `Literal[True]` are inferred as `int` instead of `bool`

---

_@pt1243_

### Summary

Apologies if this is a duplicate - I quickly skimmed the issue tracker but did not see any reports of this exact case.

When using a bitwise operator (`&`, `|`, or `^`) on a `Literal[True]` (or `Literal[False]`) and a `bool`, ty incorrectly reports the result as being of type `int`:

```python
from random import random

def f() -> bool:
    return random() > 0.5

a = True & f()
reveal_type(a)  # revealed type is int, should be bool
```

[Playground link here.](https://play.ty.dev/3b2e2206-8d8a-4703-ac69-2df11fa1d48c)

### Version

ty 0.0.1-alpha.9 (2feba5eb2 2025-06-11)

---

_Label `bug` added by @AlexWaygood on 2025-06-13 14:21_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-06-13 14:21_

---

_Comment by @AlexWaygood on 2025-06-13 14:21_

This seemed similar enough to #517 that I hoped it would be fixed by https://github.com/astral-sh/ruff/pull/18329, but that doesn't seem to be the case. So, no, not a duplicate -- thank you!

---

_Comment by @alpaylan on 2025-06-13 14:51_

I'm thinking of checking this out as a `good first issue`, does that make sense? 

---

_Comment by @alpaylan on 2025-06-13 15:21_

The issue is at https://github.com/astral-sh/ruff/blob/793ff9bdbc2ad4909f45594726ee0d2ff166130c/crates/ty_python_semantic/src/types/infer.rs#L6708

```rust
  (Type::BooleanLiteral(bool_value), right, op) => self.infer_binary_expression_type(
      node,
      emitted_division_by_zero_diagnostic,
      Type::IntLiteral(i64::from(bool_value)),
      right,
      op,
  ),
```

When pattern matching, the types are `infer_binary_expression_type: left_ty = Literal[True], right_ty = bool, op = BitAnd`

At L6708, the boolean literal `Literal[True]` is converted to integer literal `Literal[1]`, which results in the bug mentioned.


---

_Comment by @carljm on 2025-06-13 18:26_

@alpaylan Thanks for the root-causing and the fix! I reviewed your PR.

---

_Assigned to @alpaylan by @carljm on 2025-06-13 18:26_

---

_Closed by @AlexWaygood on 2025-06-17 17:02_

---
