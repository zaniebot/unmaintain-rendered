---
number: 11778
title: "linter: uncovered branches"
type: issue
state: open
author: antonio-antuan
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-06-06T18:59:57Z
updated_at: 2024-06-07T06:31:36Z
url: https://github.com/astral-sh/ruff/issues/11778
synced_at: 2026-01-07T13:12:15-06:00
---

# linter: uncovered branches

---

_Issue opened by @antonio-antuan on 2024-06-06 18:59_

I'd like to have a linter that checks that all variants are covered by match expression. An example:
```
from enum import Enum


class Foo(Enum):
    A = 1
    B = 2


def foo(f: Foo):
    var: int
    match f:
        case Foo.A:
            var = 1
    print(var)


foo(Foo.B)
```

`ruff check ./` reports that `All checks are passed`. However, if you run the code it raises UnboundLocalError.
The way to solve it is simple (I guess): check that all branches are covered. 
Definitely there can be more complex expression, like `case Foo.A | Foo.B`, `case _` which would be great to cover as well.

---

_Label `type-inference` added by @MichaReiser on 2024-06-07 06:30_

---

_Comment by @MichaReiser on 2024-06-07 06:31_

That makes sense. I don't think Ruff is capable of running this today. It will require type inference to know what `f` evaluates to, to know which branches are possible. We're working on adding support for type-inference but it will take us a while.

---

_Label `rule` added by @MichaReiser on 2024-06-07 06:31_

---
