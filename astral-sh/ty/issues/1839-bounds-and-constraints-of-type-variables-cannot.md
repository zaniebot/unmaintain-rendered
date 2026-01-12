```yaml
number: 1839
title: Bounds and constraints of type variables cannot be generic
type: issue
state: open
author: mtshiba
labels:
  - generics
assignees: []
created_at: 2025-12-10T12:28:20Z
updated_at: 2025-12-13T02:45:23Z
url: https://github.com/astral-sh/ty/issues/1839
synced_at: 2026-01-12T15:54:25Z
```

# Bounds and constraints of type variables cannot be generic

---

_@mtshiba_

### Summary

In the current Python type specification, bounds and constraints for type variables must be concrete types and cannot contain type variables (even though it's possible at runtime).

[peps.python.org/pep-0695#type-parameter-scopes](https://peps.python.org/pep-0695/#type-parameter-scopes)

So, all of the following cases should be reported as errors:

```py
from typing import TypeVar

# OK
T1 = TypeVar("T1", bound=list[int])
T2 = TypeVar("T2", list[int], str)

U = TypeVar("U")
# error
T3 = TypeVar("T3", bound=U)
# error
T4 = TypeVar("T4", bound=list[U])
# error
T5 = TypeVar("T5", bound=list["T5"])
# error
T6 = TypeVar("T6", U, str)
# error
T7 = TypeVar("T7", list["T7"], str)
```

```py
# OK
def _[T: list[int]](): ...
def _[T: (list[int], str)](): ...

# error
def _[U, T: U](): ...
# error
def _[U, T: list[U]](): ...
# error
def _[T: list[T]](): ...
# error
def _[U, T: (U, str)](): ...
# error
def _[T: (list[T], str)](): ...
```

However, this is difficult to implement straightforwardly in the current implementation of ty, because bounds/constraints on type variables are lazily evaluated.

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-12-10 12:33_

> However, this is difficult to implement straightforwardly in the current implementation of ty, because bounds/constraints on type variables are lazily evaluated.

Evaluation of class bases is also deferred, so most checks that would involve iterating over a class's bases or MRO are done after the rest of the scope has been inferred. We can probably do something similar for checking type variables? See https://github.com/astral-sh/ruff/blob/ff7086d9ad24729891754ba597ca5abc513e4858/crates/ty_python_semantic/src/types/infer/builder.rs#L581

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-10 12:36_

---

_Comment by @mtshiba on 2025-12-10 14:19_

> Evaluation of class bases is also deferred, so most checks that would involve iterating over a class's bases or MRO are done after the rest of the scope has been inferred. We can probably do something similar for checking type variables? See [astral-sh/ruff@`ff7086d`/crates/ty_python_semantic/src/types/infer/builder.rs#L581](https://github.com/astral-sh/ruff/blob/ff7086d9ad24729891754ba597ca5abc513e4858/crates/ty_python_semantic/src/types/infer/builder.rs#L581)

Ah, maybe we can use this pattern. I'll give it a try.

---

_Label `generics` added by @mtshiba on 2025-12-13 02:45_

---

_Assigned to @mtshiba by @mtshiba on 2025-12-13 02:45_

---
