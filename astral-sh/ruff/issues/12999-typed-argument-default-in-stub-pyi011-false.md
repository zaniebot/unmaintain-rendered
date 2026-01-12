```yaml
number: 12999
title: "`typed-argument-default-in-stub` (`PYI011`) - false positive when default value intentionally used to infer the default value of a generic"
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-triage
assignees: []
created_at: 2024-08-20T01:18:10Z
updated_at: 2024-08-21T06:44:57Z
url: https://github.com/astral-sh/ruff/issues/12999
synced_at: 2026-01-12T15:54:52Z
```

# `typed-argument-default-in-stub` (`PYI011`) - false positive when default value intentionally used to infer the default value of a generic

---

_@DetachHead_

not sure if this is something that can be determined without type analysis (#3893), but sometimes `PYI011` conflicts with pyright's `reportInvalidTypeVarUse` rule. for example, in the following `.pyi` file:

```py
# foo.pyi

a: int

def foo[T](value: T | None = a) -> T: ... # error: PYI011
```
```py
# usage.py

from foo import foo
reveal_type(foo()) # int
```

if we take ruff's advice and omit the default value here, it prevents type checkers from being able to infer the generic based on the default value, which is why pyright reports an error:
```py
# foo.pyi

a: int

def foo[T](value: T | None = ...) -> T: ... # no ruff error, but now pyright reports reportInvalidTypeVarUse
```
```py
# usage.py

from foo import foo
reveal_type(foo()) # Any
```

---

_Comment by @Avasam on 2024-08-20 16:06_

A couple notes:

Note that assigning a default value to a generic to infer the default generic type is currently not supported in mypy and will result in type `Never`:
```
main.py:3: error: Missing return statement  [empty-body]
main.py:3: error: Incompatible default for argument "value" (default has type "int", argument has type "T | None")  [assignment]
main.py:5: note: Revealed type is "Never"
```
Tracked here: https://github.com/python/mypy/issues/3737 (feel free to upvote, I too hit that issue and wished it worked like in pyright)

Whilst this being flagged by `PYI011` is incidental, I'd recommend avoiding that form unless you're writing private stubs (using pyright only), and instead use something like:
```py
from typing import overload

@overload
def foo() -> int: ...
@overload
def foo[T](value: T | None) -> T: ... 
```

---

Now as for `PYI011` itself, it's more of a convention in typeshed to not bother with non-literal defaults. I don't think there's anything inherently wrong with your example[^1] (assuming it works in the type-checkers you're interested in). I personally don't think it's a false-positive, you may simply not be interested in the rule for your project.

[^1]: In your example,
    ```py
    reveal_type(foo(None))
    ```
    would be `Unknown` / `Any`. But that has no effect on the mypy issue mentioned above or `PYI011`, so I'll ignore that

---

_Label `rule` added by @MichaReiser on 2024-08-21 06:44_

---

_Label `needs-triage` added by @MichaReiser on 2024-08-21 06:44_

---
