```yaml
number: 539
title: Handle SQLAlchemy fields without mapped_column()
type: issue
state: closed
author: johnniemorrow
labels:
  - metaprogramming
assignees: []
created_at: 2025-05-28T13:07:40Z
updated_at: 2025-05-30T00:23:03Z
url: https://github.com/astral-sh/ty/issues/539
synced_at: 2026-01-10T02:34:10Z
```

# Handle SQLAlchemy fields without mapped_column()

---

_Issue opened by @johnniemorrow on 2025-05-28 13:07_

### Summary

Hi All,

Something I noticed - I don't know if it's a bug, maybe more of a nice to have.

In SQLAlchemy you can declare fields with type `Mapped[<some type>]` and it'll figure out the typing. Below is an example similar to ones in https://docs.sqlalchemy.org/en/20/orm/quickstart.html with `name` as a fixed-width string (mapping to postgres varchar(4)), and `fullname` as variable string (mapping to a postgres varchar). If you don't need any specific type customization, e.g. like `fullname`, you can omit the `mapped_column()` part as shown:


```python
from sqlalchemy import String
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "user_account"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(30))
    fullname: Mapped[str]


def main():
    user = User()
    user.id = 1
    user.name = "Test"
    user.fullname = "Case"
```

ty is reporting an error for the `fullname` field assignment:

```bash
error[invalid-assignment]: Object of type `Literal["Case"]` is not assignable to attribute `fullname` of type `Mapped[str | None]`
  --> src/scratch/sqla.py:20:5
   |
18 |     user.id = 1
19 |     user.name = "Test"
20 |     user.fullname = "Case"
   |     ^^^^^^^^^^^^^
   |
info: rule `invalid-assignment` is enabled by default
```

I can get around it by adding this:

```python
fullname: Mapped[str] = mapped_column()
```

I'm not sure what the call to mapped_column() with no parameters is doing, but it would be great if ty could determine the type just from `Mapped[str]` alone, I think a lot of people will have fields defined like this in their models.

### Version

ty 0.0.1-alpha.7

---

_Comment by @MichaReiser on 2025-05-28 13:15_

I think this is the same as https://github.com/astral-sh/ty/issues/384

---

_Comment by @johnniemorrow on 2025-05-28 13:53_

The root cause may be related, but this is a different error message - here it's giving an `invalid-assignment`

---

_Comment by @AlexWaygood on 2025-05-28 13:54_

the error code is different but the two issues definitely both have in common that they both involve SQLAlchemy doing something magical with its `Mapped` annotations. It would be interesting to know if mypy and pyright support this (and if mypy does -- does it only support it if the SQLAlchemy plugin is installed? Or does it support it "out of the box"?)

---

_Comment by @johnniemorrow on 2025-05-28 14:01_

mypy is ok with it, it doesn't report any errors:

```bash
$ uv add mypy
Resolved 12 packages in 135ms
      Built scratch @ file:///home/johnnie/work/github/scratch
Prepared 2 packages in 97ms
Uninstalled 1 package in 0.78ms
Installed 3 packages in 52ms
 + mypy==1.15.0
 + mypy-extensions==1.1.0
 ~ scratch==0.1.0 (from file:///home/johnnie/work/github/scratch)
scratchscratch (main #) $ mypy src/scratch/sqla.py 
Success: no issues found in 1 source file
scratchscratch (main #) $ uv pip list
Package           Version Editable project location
----------------- ------- ---------------------------------
greenlet          3.2.2
mypy              1.15.0
mypy-extensions   1.1.0
scratch           0.1.0   /home/johnnie/work/github/scratch
sqlalchemy        2.0.41
typing-extensions 4.13.2
```

---

_Label `metaprogramming` added by @AlexWaygood on 2025-05-28 15:37_

---

_Comment by @am1ter on 2025-05-29 04:08_

I faced the same error during my `ty` tests. But I would like to add one more point here, just in case it could be helpful. Here is an example:
```python
from typing import Any

from sqlalchemy.orm import Mapped, mapped_column

from app.interfaces.orm_model import OrmModel

type JsonDictAny = dict[str, Any]


class TestOrmClass(OrmModel):
    __tablename__ = "test_orm_class"

    id: Mapped[int] = mapped_column()
    json_col: Mapped[JsonDictAny]


def main() -> None:
    session: TestOrmClass = TestOrmClass()
    session.id = 1
    session.json_col = {"ping": "pong"}
    print(len(session.json_col))
```

`ty` results:
```
error[invalid-assignment]: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `phase` of type `Mapped[dict[str, Any]]`
  --> test.py:20:5
   |
18 |     session: ValidationSessionOrm = ValidationSessionOrm()
19 |     session.id = 1
20 |     session.phase = {"ping": "pong"}
   |     ^^^^^^^^^^^^^
21 |     print(len(session.phase))
   |
info: rule `invalid-assignment` is enabled by default

error[invalid-argument-type]: Argument to function `len` is incorrect
  --> test.py:21:15
   |
19 |     session.id = 1
20 |     session.phase = {"ping": "pong"}
21 |     print(len(session.phase))
   |               ^^^^^^^^^^^^^ Expected `Sized`, found `Mapped[dict[str, Any]]`
   |
info: Function defined here
    --> stdlib/builtins.pyi:1500:5
     |
1498 | def isinstance(obj: object, class_or_tuple: _ClassInfo, /) -> bool: ...
1499 | def issubclass(cls: type, class_or_tuple: _ClassInfo, /) -> bool: ...
1500 | def len(obj: Sized, /) -> int: ...
     |     ^^^ ---------- Parameter declared here
1501 |
1502 | license: _sitebuiltins._Printer
     |
info: rule `invalid-argument-type` is enabled by default
```

In terms of pyright there are no errors:
```
0 errors, 0 warnings, 0 informations
```

---

_Comment by @carljm on 2025-05-30 00:23_

I think the underlying issue here is the same as #384 -- that we don't consider a non-ClassVar annotated on the class but with nothing assigned to it, to be present on the class (only on instances). So we don't apply the descriptor protocol to it.

Going to close this as duplicate, but the additional cases here are useful - thank you!

---

_Closed by @carljm on 2025-05-30 00:23_

---
