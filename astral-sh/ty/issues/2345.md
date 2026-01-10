```yaml
number: 2345
title: ty is unable to infer the type of sqlalchemy hybrid properties
type: issue
state: open
author: iyad-f
labels: []
assignees: []
created_at: 2026-01-05T15:12:59Z
updated_at: 2026-01-05T23:06:01Z
url: https://github.com/astral-sh/ty/issues/2345
synced_at: 2026-01-10T01:56:41Z
```

# ty is unable to infer the type of sqlalchemy hybrid properties

---

_Issue opened by @iyad-f on 2026-01-05 15:12_

### Summary

mcve
```py
from sqlalchemy.ext.hybrid import hybrid_property
from sqlalchemy.orm import DeclarativeBase


class Base(DeclarativeBase): ...


class Model(Base):
    @hybrid_property
    def foo(self) -> bool:
        return True


x = Model()
x.foo  # type inferred by ty = Unkown, expected type = bool
```

### Version

0.0.8

---

_Comment by @carljm on 2026-01-05 23:05_

Thanks! I think this has the same root cause as #1446 

---

_Added to milestone `Stable` by @carljm on 2026-01-05 23:06_

---
