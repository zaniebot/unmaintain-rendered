---
number: 1462
title: "Deducing types in generic functions (example: SQLModel)"
type: issue
state: closed
author: graipher
labels:
  - generics
assignees: []
created_at: 2025-10-31T13:25:27Z
updated_at: 2026-01-09T20:15:40Z
url: https://github.com/astral-sh/ty/issues/1462
synced_at: 2026-01-10T01:48:23Z
---

# Deducing types in generic functions (example: SQLModel)

---

_Issue opened by @graipher on 2025-10-31 13:25_

### Summary

This may just be another variant of https://github.com/astral-sh/ty/issues/623, but when using [SQLModel](https://sqlmodel.tiangolo.com/) ty cannot properly deduce the type of `session.get(Item, item_id)`:

models.py:
```python
from collections.abc import Iterator
from contextlib import contextmanager

from sqlmodel import Field, Session, SQLModel, create_engine


DATABASE_URL = "sqlite://"

engine = create_engine(
    DATABASE_URL,
    echo=False,
    connect_args={"check_same_thread": False},
)


class Item(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str


def init_db() -> None:
    """Create all tables for the demo database."""
    SQLModel.metadata.create_all(engine)


@contextmanager
def get_session() -> Iterator[Session]:
    with Session(engine) as session:
        yield session
```

main.py:
```python
from .models import Item, get_session, init_db
from typing import reveal_type

def demo_session_get(item_id: int) -> Item | None:
    with get_session() as session:
        item = session.get(Item, item_id)
        reveal_type(item)
        return item
```

Other type checkers are able to deduce the type as `Item | None`, ty just says `Unknown`:
```
$ uv run pyright
/tmp/foo/src/foo/__init__.py
  /tmp/foo/src/foo/__init__.py:20:21 - information: Type of "item" is "Item | None"
0 errors, 0 warnings, 1 information

$ uv run mypy --strict .
src/foo/__init__.py:20: note: Revealed type is "foo.models.Item | None"
Success: no issues found in 2 source files

$ uvx ty check .
info[revealed-type]: Revealed type
  --> src/foo/__init__.py:20:21
   |
18 |     with get_session() as session:
19 |         item = session.get(Item, item_id)
20 |         reveal_type(item)
   |                     ^^^^ `Unknown`
21 |         return item
   |

Found 1 diagnostic
```

The relevant parts of the typing of `Session.get` are:
```
_O = TypeVar("_O", bound=object)

class Session(_SessionClassMethods, EventTarget):
    ...

    def get(
        self,
        entity: _EntityBindKey[_O],
        ident: _PKIdentityArgument,
        *,
        ...
    ) -> Optional[_O]:
        ...
```

### Version

ty 0.0.1-alpha.25

---

_Label `generics` added by @carljm on 2026-01-09 03:17_

---

_Comment by @carljm on 2026-01-09 03:17_

Thanks for the report! Sorry this seems to have fallen through the cracks. It does still seem to reproduce.

---

_Added to milestone `Stable` by @carljm on 2026-01-09 03:18_

---

_Comment by @graipher on 2026-01-09 09:41_

Actually, this no longer seems to reproduce for me...

Slightly simplified example:

```python
from sqlmodel import Field, Session, SQLModel, create_engine
from typing import reveal_type


class Item(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)


if __name__ == "__main__":
    engine = create_engine("sqlite://")
    SQLModel.metadata.create_all(engine)
    with Session(engine) as session:
        item = session.get(Item, 1)
        reveal_type(item)
```

Gives me the correct:

```
info[revealed-type]: Revealed type
  --> src/foo/__init__.py:14:21
   |
12 |     with Session(engine) as session:
13 |         item = session.get(Item, 1)
14 |         reveal_type(item)
   |                     ^^^^ `Item | None`
   |

Found 1 diagnostic
```

### Version
ty 0.0.10


---

_Comment by @carljm on 2026-01-09 20:15_

Ah thanks! I actually did repro the original two-file version yesterday, but I failed to notice that the `from .models import ...` in `main.py` was failing (because I had made both main and models top-level modules), and that was the cause of the Unknown.

---

_Closed by @carljm on 2026-01-09 20:15_

---
