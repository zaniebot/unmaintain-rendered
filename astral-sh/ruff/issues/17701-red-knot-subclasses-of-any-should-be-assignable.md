```yaml
number: 17701
title: "[red-knot] Subclasses of Any should be assignable to `Callable`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2025-04-29T10:02:41Z
updated_at: 2025-05-01T08:18:13Z
url: https://github.com/astral-sh/ruff/issues/17701
synced_at: 2026-01-10T11:09:58Z
```

# [red-knot] Subclasses of Any should be assignable to `Callable`

---

_Issue opened by @sharkdp on 2025-04-29 10:02_

Subclasses of `Any` should be assignable to `Callable`. This should not be an error:

```py
from typing import Callable, Any


class MyMock(Any):
    pass

def f(c: Callable):
    c()

f(MyMock())  # Argument to this function is incorrect: Expected `(...) -> Unknown`, found `MyMock`
```
(https://types.ruff.rs/e0ded89d-526c-430e-a9c5-d02b8945f81d)

---

_Label `bug` added by @sharkdp on 2025-04-29 10:02_

---

_Label `help wanted` added by @sharkdp on 2025-04-29 10:02_

---

_Label `red-knot` added by @sharkdp on 2025-04-29 10:02_

---

_Added to milestone `Red Knot GA` by @sharkdp on 2025-04-29 10:02_

---

_Comment by @Kalmaegi on 2025-04-29 14:22_

When calling `is_assignable_to`, the following rule is triggered:

```rust
(Type::Instance(_), Type::Callable(_)) => {
    let call_symbol = self.member(db, "__call__").symbol;
    match call_symbol {
        Symbol::Type(Type::BoundMethod(call_function), _) => call_function
            .into_callable_type(db)
            .is_assignable_to(db, target),
        _ => false,
    }
}
```

However, according to the typing specification, subclasses of Any should be assignable to Callable (may be assignable to any type). Currently, this case is not handled explicitly, which leads to a type error when passing a subclass of Any to a parameter of type Callable.

Would it make sense to add an early check at the beginning of is_assignable_to, such as:

```rust
if self.is_subclass_of_any(db) {
    return true;
}
```

This would ensure that any subclass of Any is assignable to any type, as expected by the typing rules.

---

_Comment by @sharkdp on 2025-04-29 15:50_

We have a similar catch-all here, which is used by the instance-to-instance assignability check:

https://github.com/astral-sh/ruff/blob/c9a6b1a9d0d131c637af04977421d052b70e0e93/crates/red_knot_python_semantic/src/types/class.rs#L277-L285

Notice that this has an explicit test for `@final` classes, since a subclass of `Any` could not possibly be a subclass of a final class. We should carefully evaluate if there are any other exceptions to the rule (does it make sense for a subclass of any to be assignable to `Literal[1]`, for example?), but otherwise, introducing such a catch-all pattern sounds reasonable.

---

_Comment by @Kalmaegi on 2025-04-29 16:03_

> We have a similar catch-all here, which is used by the instance-to-instance assignability check:
> 
> [ruff/crates/red_knot_python_semantic/src/types/class.rs](https://github.com/astral-sh/ruff/blob/c9a6b1a9d0d131c637af04977421d052b70e0e93/crates/red_knot_python_semantic/src/types/class.rs#L277-L285)
> 
> Lines 277 to 285 in [c9a6b1a](/astral-sh/ruff/commit/c9a6b1a9d0d131c637af04977421d052b70e0e93)
> 
>  if self.iter_mro(db).any(|base| { 
>      matches!( 
>          base, 
>          ClassBase::Dynamic(DynamicType::Any | DynamicType::Unknown) 
>      ) 
>  }) && !other.is_final(db) 
>  { 
>      return true; 
>  } 
> Notice that this has an explicit test for `@final` classes, since a subclass of `Any` could not possibly be a subclass of a final class. We should carefully evaluate if there are any other exceptions to the rule (does it make sense for a subclass of any to be assignable to `Literal[1]`, for example?), but otherwise, introducing such a catch-all pattern sounds reasonable.

Really appreciate your quick response. I'll go through the suggested code and work on preparing the PR.

---

_Closed by @sharkdp on 2025-05-01 08:18_

---
