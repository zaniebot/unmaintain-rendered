```yaml
number: 1314
title: Missing support for SQLAlchemy ORM Select and Query
type: issue
state: closed
author: stephencsnow
labels:
  - library
assignees: []
created_at: 2025-10-07T04:29:25Z
updated_at: 2025-12-09T15:23:02Z
url: https://github.com/astral-sh/ty/issues/1314
synced_at: 2026-01-10T01:56:40Z
```

# Missing support for SQLAlchemy ORM Select and Query

---

_Issue opened by @stephencsnow on 2025-10-07 04:29_

### Summary

SQLAlchemy typing for `Select` and `Query` statements on `Mapped` columns does not appear to be working in current versions of it and `ty`.

### Expected behavior

[Docs reference](https://docs.sqlalchemy.org/en/20/changelog/whatsnew_20.html#typing-is-supported-from-step-3-onwards).

In the reproduction script below, we would expect the same revealed types as mypy.

### Reproduction

The script below is annotated with results from mypy vs ty.

Note: both pyright/pyrefly had same correct behavior as mypy.

```python
from typing import cast, reveal_type

from sqlalchemy import Select, select, Integer, Text
from sqlalchemy.engine import Row
from sqlalchemy.orm.query import Query, RowReturningQuery
from sqlalchemy.orm import Session
from sqlalchemy.orm import DeclarativeBase
from sqlalchemy.orm import Mapped, mapped_column
from sqlalchemy import create_engine


class Base(DeclarativeBase):
    pass


class User(Base):
    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    phone: Mapped[str] = mapped_column(Text)


engine = create_engine("sqlite://")
session = Session(engine)

stmt = select(User.id, User.phone)
# mypy: Select[tuple[int, str]]
#   ty: Select[tuple[Unknown, Unknown]]
reveal_type(stmt)

query = session.query(User.id, User.phone)
# mypy: RowReturningQuery[tuple[int, str]]
#   ty: RowReturningQuery[tuple[Unknown, Unknown]]
reveal_type(query)

for row in session.execute(stmt):
    # mypy: Row[Tuple[int, str]]
    #   ty: Row[tuple[Unknown, Unknown]]
    reveal_type(row)
for row in query.all():
    # mypy: Row[Tuple[int, str]]
    #   ty: Row[tuple[Unknown, Unknown]]
    reveal_type(row)  

user_stmt = select(User)
# mypy: Select[tuple[User]]
#   ty: Select[tuple[Unknown]]
reveal_type(user_stmt)

users = session.scalars(select(User)).all()
# mypy: Sequence[User]
#   ty: Sequence[Unknown]
reveal_type(users)

users_legacy = session.query(User).all()
# mypy: list[User]
#   ty: list[Unknown]
reveal_type(users_legacy)


# None of these give type errors for mypy/ty
def get_user_id_and_phone_base_stmt() -> Select[tuple[int, str]]:
    return select(User.id, User.phone)


def get_user_id_with_only_columns() -> Select[tuple[int]]:
    return get_user_id_and_phone_base_stmt().with_only_columns(User.id)


def get_user_id_and_phone() -> Row[tuple[int, str]]:
    return session.execute(get_user_id_and_phone_base_stmt()).one()


def get_user_stmt() -> Select[tuple[User]]:
    return select(User)


def get_user() -> User:
    return session.scalars(get_user_stmt()).one()


def get_user_id_and_phone_query() -> RowReturningQuery[tuple[int, str]]:
    return session.query(User.id, User.phone)


def get_user_id_with_entities() -> RowReturningQuery[tuple[int]]:
    query = get_user_id_and_phone_query().with_entities(User.id)
    # Not handled well in either, legacy pattern
    # mypy: Query[Any]
    #   ty: Query[Unknown]
    reveal_type(query)
    return cast(RowReturningQuery[tuple[int]], query)


def get_user_query() -> Query[User]:
    return session.query(User)


def get_user_from_query() -> User:
    return get_user_query().one()


# Checking that Row works if we manually specify it
def get_user_id_and_phone_query_all() -> list[Row[tuple[int, str]]]:
    return get_user_id_and_phone_query().all()


rows = get_user_id_and_phone_query_all()
reveal_type(rows[0])  # both: Row[tuple[int, str]]
reveal_type(rows[0].phone)  # both: Any
reveal_type(rows[0][0])  # both: Any
reveal_type(rows[0]._t)  # both: tuple[int, str]
```


### Pyproject

```toml
[project]
name = "ty-sqla"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "mypy>=1.18.2",
    "pyrefly>=0.36.1",
    "pyright>=1.1.406",
    "sqlalchemy>=2.0.43",
    "ty>=0.0.1a21",
]
```

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Label `library` added by @AlexWaygood on 2025-10-07 06:49_

---

_Comment by @carljm on 2025-10-07 16:01_

Thanks for the report!

I assume this is mostly #623, which is being actively worked on. But this is a very useful concise reproduction script to check ourselves against.

---

_Added to milestone `Beta` by @sharkdp on 2025-10-14 21:03_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-17 14:49_

---

_Unassigned @sharkdp by @MichaReiser on 2025-11-10 16:33_

---

_Assigned to @dcreager by @MichaReiser on 2025-11-10 16:33_

---

_Comment by @sharkdp on 2025-12-03 11:03_

It looks like the recent work on generic implicit type aliases now allows to pass some of these test cases. We reveal the expected type for all of these:

```py
# Expected: Select[tuple[User]]
user_stmt = select(User)
reveal_type(user_stmt)

# Expected: Sequence[User]
users = session.scalars(select(User)).all()
reveal_type(users)

# Expected: list[User]
users_legacy = session.query(User).all()
reveal_type(users_legacy)
```

These two test cases with two arguments remain unchanged:

```py
# Expected: Select[tuple[int, str]]
# Actual (ty): Select[tuple[Unknown, Unknown]]
stmt = select(User.id, User.phone)
reveal_type(stmt)

# Expected: RowReturningQuery[tuple[int, str]]
# Actual (ty): RowReturningQuery[tuple[Unknown, Unknown]]
query = session.query(User.id, User.phone)
reveal_type(query)
```

Looking at the first one (`select(User.id, User.phone)`), the corresponding overload for the `select` call in the first test is:

```py
@overload
def select(
    __ent0: _TCCA[_T0], __ent1: _TCCA[_T1]
) -> Select[Tuple[_T0, _T1]]: ...

# where _TCCA is an import alias for:

_TypedColumnClauseArgument = Union[
    roles.TypedColumnsClauseRole[_T],
    "SQLCoreOperations[_T]",
    Type[_T],
]

# with:

class TypedColumnsClauseRole(Generic[_T_co], SQLRole): ...

class SQLCoreOperations(Generic[_T_co], ColumnOperators, TypingOnly): ...
```

We infer the type of `User.id` argument as `InstrumentedAttribute[int]`, which is defined as

```py
class InstrumentedAttribute(QueryableAttribute[_T_co]): ...

# where:

class QueryableAttribute(
    _DeclarativeMapped[_T_co],
    SQLORMExpression[_T_co],
    interfaces.InspectionAttr,
    interfaces.PropComparator[_T_co],
    roles.JoinTargetRole,
    roles.OnClauseRole,
    sql_base.Immutable,
    cache_key.SlotsMemoizedHasCacheKey,
    util.MemoizedSlots,
    EventTarget,
): ...

# and:

class PropComparator(SQLORMOperations[_T_co], Generic[_T_co], ColumnOperators): ...

class SQLORMOperations(SQLCoreOperations[_T_co], TypingOnly): ...
```

So I guess the attributes have types that are subtypes of `SQLCoreOperations[int]` and should therefore be matched by the `SQLCoreOperations[_T]` entry in the union type of the parameters. I don't know what is specifically missing from the legacy generics solver in order to support that. We should now understand those intermediate implicit generic aliases.

---

_Unassigned @dcreager by @MichaReiser on 2025-12-05 15:48_

---

_Assigned to @sharkdp by @MichaReiser on 2025-12-05 15:48_

---

_Comment by @sharkdp on 2025-12-08 14:23_

Our current SQLAlchemy support is now tracked in [this test suite](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/external/sqlalchemy.md) (thank you @stephencsnow for the initial work here).

It looks to me like the only missing puzzle piece is https://github.com/astral-sh/ty/issues/1772 (see [this diff](https://github.com/astral-sh/ruff/pull/21814/files) for the root cause analysis). I verified this by adding `cast(Select[tuple[int, str]], …)`s around the problematic `select` calls, which makes all of the TODOs go away.

---

_Comment by @sharkdp on 2025-12-08 15:32_

> the only missing puzzle piece is https://github.com/astral-sh/ty/issues/1772

That's actually not true. We also need https://github.com/astral-sh/ty/issues/1809.

---

_Closed by @sharkdp on 2025-12-09 15:23_

---
