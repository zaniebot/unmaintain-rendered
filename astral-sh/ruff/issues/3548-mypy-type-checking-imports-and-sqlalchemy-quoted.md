---
number: 3548
title: Mypy TYPE_CHECKING imports and sqlalchemy quoted class names which prevent circular imports.
type: issue
state: closed
author: ddimmich
labels: []
assignees: []
created_at: 2023-03-15T18:49:14Z
updated_at: 2023-03-16T03:56:51Z
url: https://github.com/astral-sh/ruff/issues/3548
synced_at: 2026-01-10T01:22:42Z
---

# Mypy TYPE_CHECKING imports and sqlalchemy quoted class names which prevent circular imports.

---

_Issue opened by @ddimmich on 2023-03-15 18:49_

The below fails in ruff - often using sqlalchemy you need to provide class names as strings, to prevent circular imports.  Mypy suggests adding these imports inside an if TYPE_CHECKING block, so that it can do type inference.  Unfortunately this causes the pyflakes rules to break.  This is actually consistent with pyflakes behavior, but the discussion here suggests it could either interpret the values in the strings, or ignore f401 for imports in TYPE_CHECKING - neither of which are ideal.  https://github.com/PyCQA/pyflakes/issues/290

Do you have plans for ruff to handle this kind of scenario, or is it best to manually #noqa each line in these imports?  

ruff 0.0.256

Run with `ruff example.py` 

gives:
example.py:8:19: F401 [*] `x.SomeOtherModel` imported but unused


```
import sqlalchemy as sa
from sqlalchemy.schema import MetaData
from sqlalchemy.ext.declarative import declarative_base

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from x import SomeOtherModel

metadata = MetaData()
Base = declarative_base(metadata=metadata)

class MyModel(Base):
    data_source = sa.orm.relationship("SomeOtherModel")
```



---

_Comment by @JonathanPlasse on 2023-03-15 19:23_

`SomeOtherModel` is not used in any annotation.
Therefore ruff cannot detect it.
You need to use `SomeOtherModel`as annotation for `data_source`.

---

_Comment by @charliermarsh on 2023-03-15 19:47_

Yeah we could pick up `SomeOtherModel` if it were used as an annotation (like `data_source: "SomeOtherModel" = ...`). But as-is, we have no way of knowing that it's being used as an annotation there.

We _could_ encode that the arguments to SQLAlchemy's `relationship` function should be treated as types. I'd have to better understand those APIs and how they're intended to be used. Is there an annotation that can go on `data_source`?

---

_Comment by @JonathanPlasse on 2023-03-15 20:07_

https://docs.sqlalchemy.org/en/20/orm/extensions/mypy.html#mapping-relationships

---

_Comment by @charliermarsh on 2023-03-15 23:24_

Ah yeah, ok, I _think_ this is a valid workaround so gonna close as "expected behavior":

```py
import sqlalchemy as sa
from sqlalchemy.schema import MetaData
from sqlalchemy.ext.declarative import declarative_base

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from x import SomeOtherModel

metadata = MetaData()
Base = declarative_base(metadata=metadata)

class MyModel(Base):
    data_source: "SomeOtherModel" = sa.orm.relationship("SomeOtherModel")
```

Let me know if that's not sufficient though, happy to keep discussing.


---

_Closed by @charliermarsh on 2023-03-15 23:24_

---

_Comment by @ddimmich on 2023-03-16 03:56_

Thank you - very clear, in the case above in sqlalchemy 2 and up it would be:
```
class MyModel(Base):
    data_source: List["SomeOtherModel"] = sa.orm.relationship("SomeOtherModel")
```

Appreciate the time taken to investigate!!  

---
