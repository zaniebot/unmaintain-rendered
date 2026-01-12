```yaml
number: 6510
title: SQLAlchemy issues with TCH003
type: issue
state: closed
author: davidroeca
labels:
  - bug
assignees: []
created_at: 2023-08-11T18:33:59Z
updated_at: 2025-10-31T06:17:11Z
url: https://github.com/astral-sh/ruff/issues/6510
synced_at: 2026-01-12T15:54:46Z
```

# SQLAlchemy issues with TCH003

---

_@davidroeca_

For a full reproduction of this bug, please head [here](https://github.com/davidroeca/ruff-sqlalchemy-repro)
---
The following SQLAlchemy model written as
```python
"""The failing example."""
from __future__ import annotations

from datetime import date

from sqlalchemy.orm import Mapped, mapped_column

from .base import Base

class Birthday(Base):
    """Silly table to reproduce issue."""

    __tablename__ = "birthday"
    id: Mapped[int] = mapped_column(primary_key=True)  # noqa: A003
    day: Mapped[date]
```

and with the following ruff config:

```toml
[tool.ruff]
line-length = 79
select = ["ALL"]
ignore = []
```

and with the following ruff version:

```shell
$ ruff --version
ruff 0.0.284
```

will get reformatted with `ruff --fix model.py` in the following way:

```python
"""The failing example."""
from __future__ import annotations

from typing import TYPE_CHECKING

from sqlalchemy.orm import Mapped, mapped_column

from .base import Base

if TYPE_CHECKING:
    from datetime import date


class Birthday(Base):
    """Silly table to reproduce issue."""

    __tablename__ = "birthday"
    id: Mapped[int] = mapped_column(primary_key=True)  # noqa: A003
    day: Mapped[date]
```
Which produces an error in SQLAlchemy:
> sqlalchemy.exc.ArgumentError: Could not resolve all types within mapped annotation: "Mapped[date]". Ensure all types are written correctly and are imported within the module in use.

## My thoughts here

This rule will need to be disabled for SQLAlchemy projects that otherwise risk running into this bug. 

One consideration is that SQLAlchemy models imported as part of a Mapped[Model] relationship will need to go under the TYPE_CHECKING flag to avoid circular imports, while std lib types are not supported in this fashion. The likely cause of this is that Mapped["Model"] is supported by SQLAlchemy, while Mapped["date"] is not.

I'm not sure if there's a simple fix aside from disabling, but thought it could be worth discussing at the very least.

---

_Comment by @zanieb on 2023-08-11 20:29_

Thanks for raising!

> Mapped["Model"] is supported by SQLAlchemy, while Mapped["date"] is not.

Why is that? Should they just support `Mapped["date"]` too?

I wonder if this issue applies to Pydantic models and data classes? Should these types be exempt from the rule? (If so, we'll only be able to relatively naive false positive prevention due to our lack of strong type inference.)


---

_Label `bug` added by @zanieb on 2023-08-11 20:29_

---

_Comment by @charliermarsh on 2023-08-11 20:31_

We do support marking classes as runtime-evaluated: https://beta.ruff.rs/docs/settings/#flake8-type-checking-runtime-evaluated-base-classes. And these rules respect those settings. It's common to mark Pydantic and others as such, but you're absolutely correct that even if you add `pydantic.BaseModel` there, we can only catch direct subclasses of `pydantic.BaseModel`, and not further descendants.

---

_Comment by @zanieb on 2023-08-11 20:42_

Should we consider linking to that setting from this rule's documentation with a mention of SQLAlchemy?

---

_Comment by @charliermarsh on 2023-08-11 20:49_

Is there a common base class in SQLAlchemy that most models extend?

---

_Comment by @edgarrmondragon on 2023-08-11 21:02_

> Is there a common base class in SQLAlchemy that most models extend?

In SQLAlchemy 2.0 that'd be [`sqlalchemy.orm.DeclarativeBase`](https://docs.sqlalchemy.org/en/20/orm/mapping_api.html#sqlalchemy.orm.DeclarativeBase)

---

_Comment by @davidroeca on 2023-08-11 21:22_

> > Is there a common base class in SQLAlchemy that most models extend?

> In SQLAlchemy 2.0 that'd be [sqlalchemy.orm.DeclarativeBase](https://docs.sqlalchemy.org/en/20/orm/mapping_api.html#sqlalchemy.orm.DeclarativeBase)

Following the [SQLAlchemy docs](https://docs.sqlalchemy.org/en/20/orm/declarative_tables.html#table-configuration-with-declarative), users will derive their own shared `Base` class. Maybe worth documenting how users can mark their `Base`?

> Why is that? Should they just support Mapped["date"] too?

It would be great for SQLAlchemy to support, though I think the reason SQLAlchemy supports runtime string types is due to the support of string declarations in constructs such as `relationship("Model")` as opposed to `relationship(Model)`. It won't have the same metadata about every possible python type, but that could be worth posing to SQLAlchemy.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-14 18:57_

---

_Closed by @charliermarsh on 2023-08-14 19:47_

---

_Comment by @davidroeca on 2023-08-14 20:27_

Tried this out, and two things worth mentioning here:
1. ~Relative imports break `flake8-type-checking.runtime-evaluated-base-classes`. If I have `app/models/base.py` and write my import like `from .base import Base`, then it doesn't get ignored. Might be a separate issue, but related to this work-around.~ EDIT: this was an issue with an editor plugin of mine
2. As mentioned above, two related models in separate modules actually will need string-evaluated types rather than runtime-evaluated types. A good example would be [the one to many docs](https://docs.sqlalchemy.org/en/20/orm/basic_relationships.html#one-to-many) -- check the bidirectional implementation in the second block. Imagine `parent.py` references `child.py` and vice versa. Those imports need to be in a TYPE_CHECKING block, while normal python types need to stay outside of it. 

---

_Comment by @charliermarsh on 2023-08-14 20:29_

Relative imports should be working (did you use `app.models.base.Base`?), unless module resolution overall is not quite right on your project. Do you use import-sorting? Does it get first-party detection right?

> Those imports need to be in a TYPE_CHECKING block, while normal python types need to stay outside of it.

Why is it that they need to be imported in a `TYPE_CHECKING` block?


---

_Comment by @charliermarsh on 2023-08-14 20:44_

It may end up being an impossible constraint to meet without coupling our rule deeply to the SQLAlchemy internals, since we'd want to require that `A` in `Mapped[A]` is always available at runtime _unless_ `A` is a SQLAlchemy model in which case it should only be required at typing time.


---

_Comment by @davidroeca on 2023-08-15 00:53_

> Why is it that they need to be imported in a TYPE_CHECKING block?

Circular imports. Type checkers can get around them, but the python runtime cannot.

---

_Comment by @awoimbee on 2025-10-30 17:05_

~~There is still an issue with `@declared_attr`:~~
It's just completely broken, ~~ignoring the files~~ disabling the rules is the best solution for now

---
