```yaml
number: 1329
title: Ty misses required-field diagnostic for SQLModel (works for BaseModel)
type: issue
state: open
author: Duckling92
labels:
  - library
assignees: []
created_at: 2025-10-09T14:42:39Z
updated_at: 2025-10-31T18:10:18Z
url: https://github.com/astral-sh/ty/issues/1329
synced_at: 2026-01-12T15:54:25Z
```

# Ty misses required-field diagnostic for SQLModel (works for BaseModel)

---

_@Duckling92_

### Summary
Ty flags missing required fields for `pydantic.BaseModel` but not for `sqlmodel.SQLModel` subclasses, both using Pydantic v2.

### Minimal repro
```py
from __future__ import annotations

from pydantic import BaseModel
from sqlmodel import SQLModel


class ExamplePydantic(BaseModel):
    number: float


class ExampleSQLModel(SQLModel):
    number: float


ExampleSQLModel()  # ty: no error (bug)
ExamplePydantic()  # ty: error (good)
```

### Expected
Expected: Both `ExampleSQLModel()` and `ExamplePydantic()` are flagged as missing required field number.
Actual: Only `ExamplePydantic()` is flagged, `ExampleSQLModel()` passes.

### Environment
python: 3.11.10
sqlmodel: 0.0.27
OS: MacOS Tahoe (26.0.1) 

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Label `library` added by @AlexWaygood on 2025-10-09 15:22_

---

_Comment by @carljm on 2025-10-09 15:52_

I am pretty sure the problem here is that `SQLModel` defines `__new__` and `__init__` with very permissive signatures: https://github.com/fastapi/sqlmodel/blob/main/sqlmodel/main.py#L789-L799

And I think if we see an explicitly-defined `__new__` or `__init__`, we assume that takes precedence over the dataclass-transform-generated one. So we validate the constructor call against the permissive `__new__` and `__init__` signatures and see no error.

But in this case it looks like SQLModel is just passing the arguments through, so its `__new__` is just a wrapper, and the preferred behavior would be to still validate the arguments against the dataclass-transform-generated constructor.

But it's a little tricky to determine under exactly which conditions we should make this assumption (that a constructor with permissive signature is just a pass-through.)

---

_Comment by @carljm on 2025-10-09 16:04_

For reference, mypy and pyrefly both have the same behavior we do here (catching only the `ExamplePydantic()` error). Pyright catches both errors.

@erictraut do you happen to remember what heuristic or algorithm pyright uses here to decide that a custom constructor should be effectively ignored?

---

_Comment by @erictraut on 2025-10-09 17:16_

Pyright isn't ignoring the custom `__new__`. It's overriding it with a synthesized `__new__`. Pyright synthesizes both an `__init__` and a `__new__` method for dataclasses. In the case of `__new__`, it synthesizes a method with the signature `def __new__(cls: type[<class>], *args: Any, *kwargs: Any) -> <class>`. If it didn't synthesize a `__new__` and the dataclass didn't provide a custom `__new__`, then all calls to the constructor would be flagged as an error because `object.__new__` would be used, and it doesn't support any parameters.

I'm not convicted that pyright's behavior is correct here.

It looks like ty is not synthesizing a `__new__`. If that's the case, how do you avoid the problem with `object.__new__`? I'm guessing that you're using a heuristic to skip this check?

```python
from dataclasses import dataclass

@dataclass
class DC:
    number: int

reveal_type(DC.__new__) # def __new__(cls) -> Self@new
DC.__new__(DC, 1) # ty reports an error here
DC(1) # ty does not report an error here
```

Regardless, it would be preferable for `SQLModel` to have less permissive type annotations here.

---

_Comment by @sharkdp on 2025-10-09 17:26_

> It looks like ty is not synthesizing a `__new__`. If that's the case, how do you avoid the problem with `object.__new__`? I'm guessing that you're using a heuristic to skip this check?

Correct, we skip `object.__new__` when `__new__` is called implicitly: https://github.com/astral-sh/ruff/blob/75f3c0e8e6dcd52e8194f9831aef25fddb646685/crates/ty_python_semantic/src/types.rs#L3252

---

_Comment by @carljm on 2025-10-10 00:31_

> then all calls to the constructor would be flagged as an error because `object.__new__` would be used, and it doesn't support any parameters.

But this problem with the signature of `object.__new__` is not specific to dataclasses, so "synthesizing a `__new__` method for dataclasses" wouldn't be a general solution to the problem. Does pyright actually synthesize a `__new__` method in that form for all classes that would otherwise inherit `object.__new__`?

IIRC ty's approach (skipping `object.__new__`) is roughly based on what happens at runtime, which is that `object.__new__` actually [only enforces its signature when called on a type that overrides `__new__`](https://github.com/python/cpython/blob/main/Objects/typeobject.c#L7108) or that doesn't override `__init__`, otherwise it accepts any and all arguments without complaint.

---

_Comment by @theelderbeever on 2025-10-23 15:33_

Possibly related but `pydantic` is reporting missing required params as well which shouldn't need defined
> No arguments provided for required parameters `__pydantic_extra__`, `__pydantic_fields_set__`, `__pydantic_private__` (ty missing-argument)

---

_Comment by @sharkdp on 2025-10-27 12:19_

> Possibly related but `pydantic` is reporting missing required params as well which shouldn't need defined
> 
> > No arguments provided for required parameters `__pydantic_extra__`, `__pydantic_fields_set__`, `__pydantic_private__` (ty missing-argument)

I don't think that's related. Let's discuss pydantic separately (in https://github.com/astral-sh/ty/issues/1421 or in a new ticket, if you think this is a distinct problem from #1431)

---

_Comment by @carljm on 2025-10-30 15:13_

Given that our behavior here matches both mypy and pyrefly, and is correct given the annotations on `SQLModel.__new__`, I'm inclined to say there's nothing to be done here. But we can keep the issue open for now to collect further user reports, if any, or suggestions of heuristics we could try.

---

_Comment by @Duckling92 on 2025-10-31 14:41_

Thanks for the reply. Small correction on my side: `mypy` only flags the `SQLModel` case **when the Pydantic mypy plugin is enabled**. Without the plugin, `mypy` also lets the `SQLModel()` call pass.

With the plugin enabled, `mypy` *does* catch both cases, while `ty` only catches the plain `BaseModel` case - so the behavior still differs.

Minimal repro:

```python
from __future__ import annotations

from pydantic import BaseModel
from sqlmodel import SQLModel


class ExamplePydantic(BaseModel):
    number: float


class ExampleSQLModel(SQLModel):
    number: float


ExamplePydantic()   # E: Missing positional argument "number"
ExampleSQLModel()   # E: Missing positional argument "number"
```

`pyproject.toml`:

```ini
[tool.mypy]
plugins = ["pydantic.mypy"]
```

Command and output:

```bash
mypy example.py
example.py:15: error: Missing named argument "number" for "ExampleSQLModel"  [call-arg]
example.py:16: error: Missing named argument "number" for "ExamplePydantic"  [call-arg]
Found 2 errors in 1 file (checked 1 source file)
```

Environment:

* python 3.11.9
* pydantic 2.12.3
* sqlmodel 0.0.27
* mypy 1.18.2
* pydantic mypy plugin enabled



---

_Comment by @carljm on 2025-10-31 18:10_

Thanks for the clarification. It's not clear to me exactly what the pydantic mypy plugin is doing that helps in this case, but it's something we can look at if we ever decide to build in similar special-cased support for pydantic.

---
