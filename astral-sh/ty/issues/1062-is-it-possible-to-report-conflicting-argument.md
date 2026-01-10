```yaml
number: 1062
title: Is it possible to report conflicting-argument-forms at site of problematic function definition instead of at call site?
type: issue
state: open
author: tlauli
labels:
  - question
  - suppression
assignees: []
created_at: 2025-08-20T14:10:30Z
updated_at: 2025-08-21T14:38:26Z
url: https://github.com/astral-sh/ty/issues/1062
synced_at: 2026-01-10T02:06:24Z
```

# Is it possible to report conflicting-argument-forms at site of problematic function definition instead of at call site?

---

_Issue opened by @tlauli on 2025-08-20 14:10_

### Question

Currently, if I have one conditionally defined function that is correct, but which triggers `conflicting-argument-forms` rule, it is necessary to add  ignore to every call of the function. Would it be possible to report the violation at the function definition, so that it is ignorable at a single location? Pyright does it, and in my opinion it also makes more sense, and it is significantly easier to ignore false positives.

```
from typing import reveal_type
from ty_extensions import is_singleton

if flag:
    f = repr  # Expects a value
else:
    f = is_singleton  # Expects a type form, only location where pyright reports an error

f(int)  # ty error 1
f(int)  # ty error 2
f(int)  # ty error 3
```

### Version

0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Label `question` added by @tlauli on 2025-08-20 14:10_

---

_Label `suppression` added by @MichaReiser on 2025-08-20 15:12_

---

_Comment by @sharkdp on 2025-08-20 15:19_

Thank you for reporting this, and for the ty-specific MRE. Can you also show us how you test this with pyright (without `ty_extensions`)? And maybe tell us more about your actual use case to see if there are other reasonable solutions (e.g. making `flag` a statically-known condition)?

---

_Comment by @tlauli on 2025-08-21 07:20_

I made a function that acts like `cast` but also checks at runtime if the cast is correct. For performance reasons I want this function to degenerate to a simple `cast` while running in prod. See the code snippet:
```
from typeguard import CollectionCheckStrategy, check_type
import os

if TYPE_CHECKING or os.environ.get("ENV") == "prod":
    safe_cast = cast
else:

    def safe_cast(typ: Any, val: Any) -> Any:
        checked = check_type(val, typ, collection_check_strategy=CollectionCheckStrategy.ALL_ITEMS)
        return cast(typ, checked)  # pyright: ignore[reportInvalidTypeForm]
```

As far as I understand it, the `typeguard.check_type` treats `typ` as a value, and `cast` treats it as a type, so the else branch is problematic, as it contains both usages. Pyright reports the error at the line with return statement, while ty reports it at every call site of `safe_cast`, even though it is the same single issue of `safe_cast` being questionably defined.

To be fair, I would not be surprised if what I tried to do could be done in a better way to pass the checks, but I still think it would be better to report the error only once, instead of at every call site.

---

_Comment by @carljm on 2025-08-21 14:38_

I think there are two different errors being reported here. Based on the suppression in the extended example, pyright is reporting that the return type of `check_type` is not valid in a type expression (in the call to `cast`). (I'm not sure why ty isn't reporting that, but we probably should be.) Ty is reporting a different problem, which is that at every call site of `safe_cast`, you are trying to call a union of two functions, and those functions don't agree about whether the second argument should be a value or type expression, so we don't know how to interpret the argument expression at each callsite. That's not a problem with either of the two definitions of `safe_cast` in isolation, it's only a problem when you try to call a union of them.

I'm working on some changes that should allow us to infer a given expression twice, once as value expression and once as type expression. Which might allow us to eliminate `conflicting-argument-forms` rule entirely. I'd recommend holding off on this issue for a bit and see what develops there.



---
