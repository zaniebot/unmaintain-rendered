---
number: 14140
title: False-positive TCH002 for runtime-required type annotations
type: issue
state: open
author: OlehChyhyryn
labels:
  - rule
assignees: []
created_at: 2024-11-06T19:55:04Z
updated_at: 2025-11-01T14:27:42Z
url: https://github.com/astral-sh/ruff/issues/14140
synced_at: 2026-01-10T01:22:54Z
---

# False-positive TCH002 for runtime-required type annotations

---

_Issue opened by @OlehChyhyryn on 2024-11-06 19:55_

In our project, we are using https://injector.readthedocs.io/en/latest/ library to implement dependency injection.

For proper workflow, it requires the following reference of injecting types: 

```
@inject
def fun(t: SomeType) -> None:
    pass
    
# or equivalent

def fun(t: Inject[SomeType]) -> None:
    pass
```

(link to the documentation - https://injector.readthedocs.io/en/latest/api.html#injector.Inject)

Both methods (type annotation and regular annotation) require that the context (`Inject` itself or any type underneath) be available during runtime.

According to the documentation, I added the required annotations to the `flake8-type-checking` configuration.

```
[tool.ruff.lint.flake8-type-checking]
runtime-evaluated-base-classes = [
    "pydantic.BaseModel",
    "injector.Inject", # DI injection library
    "injector.ClassAssistedBuilder" # DI injection library
]
```

But it does not work as expected and returns false positive warnings. 

You can reproduce it using the following sanitized sample:

```
from __future__ import annotations

from typing import Any

from injector import Inject

from services.service_a import ServiceA
from services.service_b import ServiceB



class SomeTask:
    """
    Task to pre-heat public URLs cache.
    """

    def __init__(
        self,
        *args: Any,
        some_other_variable: str,
        service_a: Inject[ServiceA],
        service_b: Inject[ServiceB],
    ) -> None:
        self._service_a = service_a
        self._service_b = service_b
        self._some_other_variable = some_other_variable

```
After running a ruff check for this file, I get the following response:

```
sample_sanitized.py:24:22:   TCH002 Move third-party import `injector.Inject` into a type-checking block
   |
22 | from typing import Any
23 | 
24 | from injector import Inject
   |                      ^^^^^^ TCH002
25 | 
26 | from services.service_a import ServiceA
   |
   = help: Move into type-checking block

sample_sanitized.py:26:32: TCH002 Move third-party import `services.service_a.ServiceA` into a type-checking block
   |
24 | from injector import Inject
25 | 
26 | from services.service_a import ServiceA
   |                                ^^^^^^^^ TCH002
27 | from services.service_b import ServiceB
   |
   = help: Move into type-checking block

sample_sanitized.py:27:32: TCH002 Move third-party import `services.service_b.ServiceB` into a type-checking block
   |
26 | from services.service_a import ServiceA
27 | from services.service_b import ServiceB
   |                                ^^^^^^^^ TCH002
   |
   = help: Move into type-checking block

Found 3 errors.
No fixes available (3 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

This issue is reproducible not only for exactly this library but for any typing annotations evaluated at runtime in similar fashion 

---

_Renamed from "False TCH002 for runtime-required type annotations" to "False-positive TCH002 for runtime-required type annotations" by @OlehChyhyryn on 2024-11-06 20:02_

---

_Comment by @MichaReiser on 2024-11-06 21:49_

Thanks for the nice write up! 

This sounds related to https://github.com/astral-sh/ruff/issues/13713

---

_Label `rule` added by @MichaReiser on 2024-11-06 21:49_

---

_Comment by @charliermarsh on 2024-11-09 02:33_

Did you try using [`runtime-evaluated-decorators`](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_runtime-evaluated-decorators) with `injector.inject`? You might have better luck there. `runtime-evaluated-base-classes` definitely doesn't work for annotations (e.g., `Inject[SomeType]`); it's applied to the class body when the mentioned class is a base class.

---

_Comment by @OlehChyhyryn on 2024-11-18 13:05_

Sorry, I was on vacation, so I could not answer quickly.

Yes, I tested this (and checked again today) and got the same result as I described in the issue description

UPD:

Let me check it with the 0.7.4 release 

---

_Comment by @OlehChyhyryn on 2024-11-18 13:15_

Checked again against https://github.com/astral-sh/ruff/releases/tag/0.7.4

The same results were obtained after adding the following to the project.toml:

```
[tool.ruff.lint.flake8-type-checking]
runtime-evaluated-base-classes = [
    "pydantic.BaseModel",
    "injector.Inject", # DI injection library
    "injector.ClassAssistedBuilder" # DI injection library
]
runtime-evaluated-decorators = [
    "injector.Inject", # DI injection library
    "injector.ClassAssistedBuilder" # DI injection library
]
```

---

_Comment by @charliermarsh on 2024-11-18 13:43_

To clarify, I believe you want:

```toml
[tool.ruff.lint.flake8-type-checking]
runtime-evaluated-decorators = [
    "injector.inject",
]
```

Notice the lower-case `inject`.

---

_Comment by @OlehChyhyryn on 2024-11-18 15:31_

I checked again with this one. Still working not as intended:

I have specified pyproject.toml as 

```
[tool.ruff.lint.flake8-type-checking]
runtime-evaluated-base-classes = [
    "injector.Inject", # DI injection library
    "injector.ClassAssistedBuilder", # DI injection library
    "injector.inject", # DI injection library
]
runtime-evaluated-decorators = [
    "injector.inject", # DI injection library
    "injector.Inject", # DI injection library
    "injector.ClassAssistedBuilder" # DI injection library
]
```

Checked for both versions of the declaration:

`@annotation` scenario is working properly.

In a scenario where only typing annotation is used, e.g., Inject[ServiceA], the error still exists. 

```
from __future__ import annotations

from typing import Any

from injector import Inject
from services.service_a import ServiceA
from services.service_b import ServiceB


class SomeTask:
    """
    Task to pre-heat public URLs cache.
    """

    @inject
    def __init__(
        self,
        *args: Any,
        some_other_variable: str,
        service_a: Inject[ServiceA],
        service_b: Inject[ServiceB],
    ) -> None:
        self._service_a = service_a
        self._service_b = service_b
        self._some_other_variable = some_other_variable
```

This one still generates false positives. 





---

_Comment by @OlehChyhyryn on 2025-03-18 20:21_

Sorry for bumping it up; the reference task (https://github.com/astral-sh/ruff/issues/13713) has been closed.

But this error (with runtime-required) annotation is still reproducible. I checked it with Ruff 0.11.0

```
[tool.ruff.lint.flake8-type-checking]
# Add quotes around type annotations, if doing so would allow
# an import to be moved into a type-checking block.
quote-annotations = true
runtime-evaluated-base-classes = [
    "injector.Inject", # DI injection library
]
exempt-modules = ["injector"]
```

```
from __future__ import annotations

from typing import Any

from injector import Inject
from services.service_a import ServiceA
from services.service_b import ServiceB


class SomeTask:
    """
    Task to pre-heat public URLs cache.
    """

    def __init__(
        self,
        *args: Any,
        some_other_variable: str,
        service_a: Inject[ServiceA],
        service_b: Inject[ServiceB],
    ) -> None:
        self._service_a = service_a
        self._service_b = service_b
        self._some_other_variable = some_other_variable
```

```
src/example.py:3:20: TC003 Move standard library import `typing.Any` into a type-checking block
  |
1 | from __future__ import annotations
2 |
3 | from typing import Any
  |                    ^^^ TC003
4 |
5 | from injector import Inject
  |
  = help: Move into type-checking block

src/example.py:6:32: TC002 Move third-party import `services.service_a.ServiceA` into a type-checking block
  |
5 | from injector import Inject
6 | from services.service_a import ServiceA
  |                                ^^^^^^^^ TC002
7 | from services.service_b import ServiceB
  |
  = help: Move into type-checking block

src/example.py:7:32: TC002 Move third-party import `services.service_b.ServiceB` into a type-checking block
  |
5 | from injector import Inject
6 | from services.service_a import ServiceA
7 | from services.service_b import ServiceB
  |                                ^^^^^^^^ TC002
  |
  = help: Move into type-checking block

Found 3 errors.
No fixes available (3 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

The expected behaviour of Injector type annotation is the fact annotation itself and everything under it I available in the runtime 

---

_Comment by @ntBre on 2025-03-18 21:30_

Thanks for the reminder. Here's a reproduction in the [playground](https://play.ruff.rs/8a302582-5c3a-4e9f-9f88-88d529ef9eaa), which might be a little easier for others to try out.

@Daverball do you know if this is resolved by any of your open PRs? I know you've been working on these rules and would know better than me.

---

_Comment by @Daverball on 2025-03-18 21:48_

No, but it's tangentially related to one of my open issues, so I plan on tackling this issue eventually: #16412

We would definitely need a new setting, so we can treat certain generics as always runtime required.

What makes this particular case a lot trickier however, is that generally it would not be enough just to treat this generic and anything it encloses as runtime required, since most naïve uses of runtime type information will use `inspect.get_annotations` or `typing.get_type_hints` both of which will fail if not all the symbols in all of the annotations are available at runtime. This will change however in Python 3.14 with PEP 649, which allows partial evaluation of `__annotations__`, that replaces anything it can't resolve with a `ForwardRef` object. There are already some runtime type libraries that go out of their way to be more forgiving. So treating everything else as runtime required would not be entirely accurate either. Which is why I believe we would also need to mark those type expressions as runtime ambiguous, i.e. neither treating them as runtime required, nor typing only. This would also involve first having to peek at all of the annotations of a class or function definition so we can check whether or not there's a runtime required generic in play.

A workaround for this that currently works is to define a no-op decorator and add it to `runtime-evaluated-decorators`, so you can manually mark functions that contain annotations that are runtime required.

---

_Comment by @omgMath on 2025-10-20 07:04_

I think I have a very similar case in relation to `fastapi`: [Playground reproduction](https://play.ruff.rs/80ba32e0-9946-4250-8392-24124f491981)

The function is used like so

```python
app.include_router(
    router,
    dependencies=[Depends(validate_is_readwrite)],
)
```

Somehow the `User` type is needed during runtime (e.g when accessing the swagger docs). 
Would be cool if both the `inject` as well as this case could be fixed with the same setting.


---

_Comment by @extrange on 2025-11-01 08:44_

Just to add on, I'm seeing this issue in Pydantic as well, where the class is needed at runtime for validation of the `BaseModel`.

```py
import pytest
from pydantic import BaseModel
from some_module import SomeClass # This cannot be moved to a TYPE_CHECKING block


class A(BaseModel):
    para: SomeClass
```

[Playground](https://play.ruff.rs/8d44bc90-2f58-470f-8801-d2f03a52f04a)

Edit: fixed with `runtime-evaluated-base-classes`

---

_Comment by @Daverball on 2025-11-01 09:01_

@extrange You need to add `pydantic.BaseModel` to `runtime-evaluated-base-classes`, then it works as expected. Pydantic is unproblematic if all the required classes are listed in `runtime-evaluated-base-classes`, it's SQLAlchemy which doesn't quite work correctly whichever way you configure it.

---
