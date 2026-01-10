---
number: 13713
title: "[TCH001] Improvement with FastAPI dependencies"
type: issue
state: closed
author: JP-Ellis
labels:
  - rule
assignees: []
created_at: 2024-10-11T06:55:14Z
updated_at: 2025-02-18T23:10:48Z
url: https://github.com/astral-sh/ruff/issues/13713
synced_at: 2026-01-10T01:22:54Z
---

# [TCH001] Improvement with FastAPI dependencies

---

_Issue opened by @JP-Ellis on 2024-10-11 06:55_

FastAPI makes extensive use of the type annotations at runtime, which then requires that the relevant imports be available at runtime. This can cause issues when Ruff suggests they be moved into type-checking blocks.

Ruff has some configuration options which allows for these imports to be excluded from the type-checking block, such as:

- [`runtime-evaluated-base-classes`](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_runtime-evaluated-base-classes)
- [`runtime-evaluated-decorators`](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_runtime-evaluated-decorators)

One place which I think isn't covered is the argument to `Depends` and `Security` which allows FastAPI to execute functions in order to perform argument injection.

E.g.

```python
def get_foo():
    return "foo"
```

```python
from .foo import get_foo
from fastapi import Depends
from typing import Annotated

def repeat_foo(foo: Annotated[str, Depends(get_foo)]):
    return foo + foo
```

Now I may have missed an option that can then tell ruff that arguments to `Depends` should _not_ be moved into the type-checking block. But if it is not currently possible, this may be a good improvement to add to ruff.


---

_Comment by @jonyscathe on 2024-10-14 05:51_

Have the exact same issue.  
I have something like:

```
from __future__ import annotations

from typing import Annotated

from fastapi import Depends, FastApi
from sqlmodel import Session

from .database import get_session
from .models import test_model

app = FastAPI()

@app.post('/test/', response_model=test_model.TestResponse)
def test(
    *,
    session: Annotated[Session, Depends(get_session)],
    test: test_models.TestRequest,
) -> test_models.TestResponse:
    return test_models.TestResponse()
```

This flags TCH001 or TCH0002 on `Depends`, `Session` and `get_session` and in reality all three are required.

Putting any of these in a type checking block results in runtime errors.
`runtime-evaluated-decorators = ["app.post"]` has no impact and even if it did I need to use the same Depends call on functions that are not routes.


Ways to fix:
* Could add a list of dependencies that TCH001/TCH002 will never flag on
* Could add a list of functions that would mean TCH001/TCH002 won't flag on that function's annotations
* Could somehow detect when things like `Depends` and `Security` are used and not flag them or their arguments (or their annotated types...)

---

_Label `rule` added by @MichaReiser on 2024-10-14 14:13_

---

_Comment by @MichaReiser on 2024-10-14 14:13_

Thanks. THis makes sense to me. 

---

_Comment by @pySilver on 2024-10-25 17:14_

@jonyscathe @MichaReiser did you guys find any solution beside moving `app` declaration into a submodule? 

---

_Comment by @MichaReiser on 2024-10-26 08:09_

> @jonyscathe @MichaReiser did you guys find any solution beside moving `app` declaration into a submodule?

I'm not aware of any solution for this issue. I understand that we need a new setting to configure runtime-evaluated type annotations

---

_Comment by @pySilver on 2024-10-28 12:35_

@MichaReiser yeah, it appears that that is the only solution. 

---

_Referenced in [astral-sh/ruff#13926](../../astral-sh/ruff/issues/13926.md) on 2024-10-28 12:40_

---

_Comment by @pySilver on 2024-10-28 12:47_

For anyone looking at this, the only solution until we get something better is to:
1. Extract your `thing` declaration into a separate module
2. Mention the decorator in `runtime-evaluated-decorators`

So for example move `app = FastAPI()` into `apps.py` and import it from there while adding `myproject.myapp.apps.app.post` into `runtime-evaluated-decorators`

Or disable this checks for now.

---

_Referenced in [astral-sh/ruff#14140](../../astral-sh/ruff/issues/14140.md) on 2024-11-06 21:49_

---

_Comment by @Daverball on 2024-11-07 07:47_

I think one sensible and easy-to-implement solution for this particular case is to special-case `Annotated` and treat itself and everything past the first element in the subscript as runtime context, since `Annotated` is almost always used for runtime introspection. Of course that stops working as soon as you move this out into a `TypeAlias` in a separate module, so it would still probably be a good idea to add a third setting for symbols that always need to be available at runtime, so corresponding imports would never emit TCH001-TCH003.

But you will also need to take into account that with functions `typing.get_type_hints` will fail if any of the parameter or return annotations contain forward references to symbols within `TYPE_CHECKING` blocks, so if you get any hits within a function, you would likely have to retroactively treat all references as runtime references, which may be quite tricky to accomplish. You would likely have to eagerly go through all the annotations for every function to see if there are any matches so you can set a flag before traversing the annotation nodes on the function for semantic analysis, which would slow things down quite a bit.

The same is technically true for classes, but there, you usually can rely on the existing settings. It's also worth pointing out that this should eventually be less of a problem once PEP-649 arrives, since it will extend `typing.get_type_hints` with the ability for partial failure, so failures in resolving part of one of the annotations will not spread to the rest of the annotation and all the other annotations on the same object. We may see earlier adoption of this improvement thanks to `typing_extensions` likely containing a backport.

Also not every runtime library will use `typing.get_type_hints`, there's quite a few that implement their own logic, so the failure conditions are not always the same. That's why in `flake8-type-checking` I also have the concept of a soft-runtime use, i.e. we treat it as neither required nor not required and just never emit an error in either case, trusting that you know what you are doing.

One good example where this is necessary is SQLAlchemy's `Mapped` which can refer to other models in relationships and these sometimes need to be forward references without a runtime import in order to break import cycles, but everything other than models needs to be available at runtime, so there's no "one-size fits all" solution there. With red knot we could eventually know which symbols refer to other SQLAlchemy models and make a more narrow exception, but at that point it would become a very targeted extension specific to SQLAlchemy.

---

_Comment by @Daverball on 2024-11-13 08:48_

Turns out ruff already properly clears the `TYPE_DEFINITION` flag for the second argument of `Annotated`, however there are additional flags for string annotations with a `__future__` import and for the runtime context of the annotation which don't get cleared.

Not clearing those flags makes sense to me, since they provide important context, however we never check for `TYPE_DEFINITION` on `is_typing_reference`, so we treat `Depends` and `get_foo` as typing references, despite not being part of a type definition. Technically this is once again somewhat correct (especially for the `from __future__ import annotations` case), however it's not really helpful to treat those references that way, so it's better to never treat non-type-definitions as typing references (unless they're within a type checking block).

I'll get a PR up with a fix.

---

_Referenced in [astral-sh/ruff#14311](../../astral-sh/ruff/pulls/14311.md) on 2024-11-13 09:00_

---

_Referenced in [astral-sh/ruff#15060](../../astral-sh/ruff/pulls/15060.md) on 2024-12-19 12:27_

---

_Comment by @JP-Ellis on 2025-02-18 23:10_

I believe this has been fixed 🎉

At least, the example I had in the original post no longer raises any linting errors as of ruff 0.9.6.

---

_Closed by @JP-Ellis on 2025-02-18 23:10_

---
