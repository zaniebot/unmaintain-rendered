---
number: 18992
title: Prohihit TC003 for types used in Pydantic
type: issue
state: closed
author: jensenbox
labels:
  - question
assignees: []
created_at: 2025-06-27T18:16:02Z
updated_at: 2025-07-07T12:48:04Z
url: https://github.com/astral-sh/ruff/issues/18992
synced_at: 2026-01-07T13:12:16-06:00
---

# Prohihit TC003 for types used in Pydantic

---

_Issue opened by @jensenbox on 2025-06-27 18:16_

### Summary

TC003 is moving out type hints that are needed by Pydantic at runtime into the TYPE_CHECKING section.

For example:

    from decimal import Decimal
    from pydantic import BaseModel

    class SomeClass(BaseModel):
        amount: Decimal


TC003 will end up moving the Decimal import into a TYPE_CHECKING block:

    if TYPE_CHECKING:
        from decimal import Decimal

The net result is a somewhat odd looking error:

    PydanticUserError: 'SomeClass' is not fully defined; you should define 'Decimal', then call 'SomeClass.model_rebuild()'

The current fix is to make the import look like:

    from decimal import Decimal  # noqa: TC003

---

_Comment by @MeGaGiGaGon on 2025-06-27 18:51_

Reproduction targeting 3.14: https://play.ruff.rs/4570a03d-be13-4ec7-a411-58f78225c3f2
```py
from decimal import Decimal
from pydantic import BaseModel

class SomeClass(BaseModel):
    amount: Decimal
```
Per the [docs on `TC003`](https://docs.astral.sh/ruff/rules/typing-only-standard-library-import/):
> If a class _requires_ that type annotations be available at runtime (as is the case for Pydantic, SQLAlchemy, and other libraries), consider using the [`lint.flake8-type-checking.runtime-evaluated-base-classes`](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_runtime-evaluated-base-classes) and [`lint.flake8-type-checking.runtime-evaluated-decorators`](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_runtime-evaluated-decorators) settings to mark them as such. 

Adding `"pydantic.BaseModel"` to `runtime-evaluated-base-classes` fixes the error: https://play.ruff.rs/ddc838a9-205b-40d7-a3c4-be0f6ba2fa5e

---

_Label `question` added by @ntBre on 2025-06-30 13:03_

---

_Comment by @ntBre on 2025-06-30 13:05_

Thanks @MeGaGiGaGon! I think `runtime-evaluated-base-classes` is the intended fix here, as you said.

---

_Closed by @MichaReiser on 2025-07-07 12:48_

---
