```yaml
number: 557
title: Daily property test run failed on Sat May 31 2025
type: issue
state: closed
author: github-actions
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-05-31T12:06:01Z
updated_at: 2025-06-10T20:25:10Z
url: https://github.com/astral-sh/ty/issues/557
synced_at: 2026-01-12T15:54:23Z
```

# Daily property test run failed on Sat May 31 2025

---

_@github-actions_

Run listed here: https://github.com/astral-sh/ty/actions/runs/15363461266

---

_Label `bug` added by @github-actions[bot] on 2025-05-31 12:06_

---

_Label `type properties` added by @github-actions[bot] on 2025-05-31 12:06_

---

_Comment by @AlexWaygood on 2025-05-31 12:17_

```
 ---- types::property_tests::stable::subtype_of_implies_not_disjoint_from stdout ----

thread 'types::property_tests::stable::subtype_of_implies_not_disjoint_from' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [Callable { params: List([]), returns: Some(TypingLiteral) }, TypingLiteral], neg: [] }, KnownClassInstance(SpecialForm))
```

I think this is because we should be simplifying a `Callable & TypeOf[Literal]` intersection to `Never` (because `TypeOf[Literal]` is not a `Callable` type), but we're not:

```py
from typing import Callable, Literal
from ty_extensions import TypeOf, Intersection

def f(x: Intersection[Callable, TypeOf[Literal]]):
    reveal_type(x)  # revealed: `((...) -> Unknown) & typing.Literal`
```

https://play.ty.dev/7145cb6b-7092-4bb3-9b07-768b42548843

---

_Closed by @carljm on 2025-06-10 20:25_

---
