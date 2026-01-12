```yaml
number: 467
title: "Type checker doesn't recognize SQLAlchemy 2.0 features (Mapped, mapped_column) despite correct installation"
type: issue
state: closed
author: y3bishop3y
labels:
  - needs-mre
assignees: []
created_at: 2025-05-20T20:49:56Z
updated_at: 2025-05-20T21:32:30Z
url: https://github.com/astral-sh/ty/issues/467
synced_at: 2026-01-12T15:54:23Z
```

# Type checker doesn't recognize SQLAlchemy 2.0 features (Mapped, mapped_column) despite correct installation

---

_@y3bishop3y_

# Description

When using ty with SQLAlchemy 2.0, it fails to recognize the `Mapped` and `mapped_column` types, which are core features of SQLAlchemy 2.0's ORM.

Error Message:

```
error[unresolved-import]: Module `sqlalchemy.orm` has no member `mapped_column`
  --> backend/db/models/packet.py:35:45
   |
33 | from sqlalchemy import (BigInteger, Boolean, DateTime, Float, ForeignKey,
34 |                         Integer, SmallInteger, String, func, select)
35 | from sqlalchemy.orm import Mapped, Session, mapped_column
   |                                             ^^^^^^^^^^^^^
36 |
37 | from backend.db.utils.query_utils import (FilterCondition, FilterOperator,
   |
info: rule `unresolved-import` is enabled by default
```

### Environment Details

Ty version: [your version here]
SQLAlchemy version: 2.0.41
Dependency management: Poetry
Python version: [your version here]

### Verification Steps

I've verified that these types do exist in my environment:

```bash
$ echo "from sqlalchemy.orm import Mapped, mapped_column; print(Mapped, mapped_column)" > test_sqlalchemy.py
$ python test_sqlalchemy.py
<class 'sqlalchemy.orm.base.Mapped'> <function mapped_column at 0x10389ee80>
```

### Additional Information

There are no type stubs available for SQLAlchemy 2.0:

```
$ poetry add --dev "types-sqlalchemy>=2.0.0"
Could not find a matching version of package types-sqlalchemy
```

SQLAlchemy 2.0 includes its own type annotations, which should be detected by Ty

### Current Workaround

Currently using type ignore comments:

```python
from sqlalchemy.orm import Mapped, Session, mapped_column  # type: ignore[attr-defined]
```

Any guidance on how to properly configure ty to recognize SQLAlchemy 2.0 types would be appreciated.

---

_Comment by @carljm on 2025-05-20 21:08_

What is likely happening here is that ty is finding a Python environment different from the poetry environment, and the Python environment ty is finding has an older version of SQLAlchemy installed in it.

I've confirmed that if I create a poetry project with a dependency on SQLAlchemy 2, and then run ty in such a way that it can discover the poetry environment, it has no trouble finding e.g. `sqlalchemy.orm.mapped_column`. Ways that I've tested that work:

* Add ty as a poetry dev dependency and then run it as `poetry run ty check` -- this ensures that poetry sets `VIRTUAL_ENV` to point to the Poetry env, and ty uses `VIRTUAL_ENV` to find your Python environment.
* Manually use `$(poetry env activate)` to activate Poetry's venv, which has the same effect, and then run ty.
* Manually point ty to the Poetry env using ty's `--python` flag (this is inconvenient since Poetry hides its envs in a distant cache location with a long and obscure path, but it works.)

The full documentation for ty's Python environment discovery is at https://github.com/astral-sh/ty/blob/main/docs/README.md#python-environment

Closing this for now since it doesn't seem to be a ty issue, but feel free to reopen if you have more detailed information that suggests it might be!

---

_Label `needs-mre` added by @carljm on 2025-05-20 21:08_

---

_Closed by @carljm on 2025-05-20 21:32_

---

_Reopened by @carljm on 2025-05-20 21:32_

---

_Closed by @carljm on 2025-05-20 21:32_

---
