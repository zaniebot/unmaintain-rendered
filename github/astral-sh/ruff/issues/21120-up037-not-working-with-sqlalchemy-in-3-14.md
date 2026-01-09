---
number: 21120
title: UP037 not working with SQLAlchemy in 3.14
type: issue
state: closed
author: extrange
labels:
  - bug
assignees: []
created_at: 2025-10-29T04:26:28Z
updated_at: 2025-11-01T12:05:31Z
url: https://github.com/astral-sh/ruff/issues/21120
synced_at: 2026-01-07T13:12:16-06:00
---

# UP037 not working with SQLAlchemy in 3.14

---

_Issue opened by @extrange on 2025-10-29 04:26_

### Summary

For the following example in Python 3.14:

```python
from typing import TYPE_CHECKING
from uuid import UUID

from sqlalchemy import ForeignKey, Uuid
from sqlalchemy.orm import Mapped, mapped_column, relationship

from app.db.schema.base import Base

if TYPE_CHECKING:
    from .user import User


class Message(Base):
    __tablename__ = "message"

    content: Mapped[str]
    user_id: Mapped[UUID] = mapped_column(Uuid, ForeignKey("user.id"))

    # This line
    user: Mapped["User"] = relationship(back_populates="messages", init=False)
```

The suggestion is to remove the quotes around `"User"`:

```sh
UP037 [*] Remove quotes from type annotation
  --> src/app/db/schema/message.py:18:18
   |
16 |     content: Mapped[str]
17 |     user_id: Mapped[UUID] = mapped_column(Uuid, ForeignKey("user.id"))
18 |     user: Mapped["User"] = relationship(back_populates="messages", init=False)
   |                  ^^^^^^
   |
help: Remove quotes
```

However doing so results in `NameError: name 'User' is not defined` at runtime. Perhaps annotations within the `Mapped` class should be excluded from this rule?

Ruff `0.14.1`

---

_Comment by @ntBre on 2025-10-29 14:58_

Ooh, thank you! This sounds related to #20782, which needed an MRE. cc @dylwil3 

---

_Label `bug` added by @ntBre on 2025-10-29 14:58_

---

_Comment by @dylwil3 on 2025-10-30 01:50_

Hmm... this seems very related to some of the discussion in here https://github.com/python/cpython/issues/130881, but I will have to read it over carefully, especially since that issue was apparently resolved.

In general it seems that the assumption that "most people will not do things with annotations that will reveal the difference between 3.14 default behavior and `from __future__ import annotations`" is correct. Unfortunately, the assumption that "also no one will use third-party libraries that do these things" is wrong.

---

_Comment by @extrange on 2025-10-30 03:44_

Just to add on, `from __future__ import annotations` is planned to be [deprecated](https://peps.python.org/pep-0749/#specification), and the default behavior will be as PEP 649.

---

_Comment by @dylwil3 on 2025-10-30 20:14_

> this will happen no sooner than the first release after Python 3.13 reaches its end-of-life, but the community may decide to wait longer.

EOL for 3.13 is end of 2029, so unfortunately I don't think we can wait it out 😄 


---

_Comment by @Daverball on 2025-10-31 12:13_

This is loosely related to a long standing issue I have been meaning to fix and the main reason why I haven't turned on the `TC` rules yet in ruff. `flake8-type-checking` has built in special casing for SQLAlchemy that works around this issue.

The main issue with SQLAlchemy's `Mapped` is, that it's using some magic to resolve forward references to other models, since it's quite common for models to reference one another which quickly leads to circularity issues if you were to actually import the model at runtime in both modules, but other than models all symbols in the annotation need to be available at runtime. So it's this weird mixed case where we can treat the symbols as neither runtime required nor purely typing related. This will need a new execution state in the semantic model I've been calling "runtime ambiguous" where neither the rules that want to add, nor the ones that want to remove quotes trigger (and also no moving around of imports in or out of type checking blocks).

---

That being said, I believe this could be considered a UX bug in SQLAlchemy's Python 3.14 support. I believe they should be relying on new `annotationlib` functionality in 3.14, namely the [`FORWARDREF`](https://docs.python.org/3/library/annotationlib.html#annotationlib.Format.FORWARDREF) mode for `get_annotations`, so unresolvable symbols are replaced by a `ForwardRef`, rather than throwing a `NameError`. That way they can apply the same partial analysis of the annotations they are already doing, and only later replacing the forward references with the actual models, once they've been mapped. Rather than force people to explicitly quote forward references to models.

Arguably it's still valid for `UP037` to trigger for quoted runtime ambiguous annotations if they're deferred, so adding "runtime ambiguous" would not actually solve this issue, although I think the more pragmatic stance would be to allow quoted annotations in "runtime ambiguous" expressions, since how the annotations get accessed are highly dependent on the library that's being used, so we can't assume that the quotes are actually redundant.

---

_Comment by @Daverball on 2025-10-31 12:21_

Looking more closely at the changes SQLAlchemy made in preparation for 3.14 I believe that's already what they're doing, so technically the above code should work on 3.14 with the latest version of SQLAlchemy.

@extrange Which version of SQLAlchemy are you using?

---

_Comment by @dylwil3 on 2025-10-31 12:22_

Thanks for the analysis and context!

> @extrange Which version of SQLAlchemy are you using?

Even better: could you provide an MRE? I wasn't able to easily reconstruct one from the code snippet you provided.

---

_Comment by @Daverball on 2025-10-31 12:31_

For additional context: SQLAlchemy 2.0.41 is the oldest version which appears to have support for PEP-649 in 3.14, although it's probably been refined in the three follow-up releases. So I'd definitely suggest testing this code with 2.0.44

If this still happens on 2.0.44 I would consider opening a bug report in the SQLAlchemy repository, since they clearly want this to work in 3.14, without you having to add quotes to your annotations.

---

_Comment by @dylwil3 on 2025-10-31 12:58_

Ok I was able to _almost_ reproduce the issue with some help from Claude to mock up the contents of `user`, `base`, etc. (So apologies if there's some kind of slop in there - I'm not super familiar with `sqlalchemy`).

```console
❯ tree
.
├── pyproject.toml
├── README.md
├── src
│   └── app
│       ├── __init__.py
│       └── db
│           ├── __init__.py
│           └── schema
│               ├── __init__.py
│               ├── base.py
│               ├── message.py
│               └── user.py
└── uv.lock

8 directories, 14 files

❯ uv tree
Resolved 4 packages in 3ms
app v0.1.0
└── sqlalchemy v2.0.44
    └── typing-extensions v4.15.0
```

with contents:

```python
# src/app/db/schema/base.py
from sqlalchemy.orm import DeclarativeBase, MappedAsDataclass


class Base(MappedAsDataclass, DeclarativeBase):
    pass
```

```python
# src/app/db/schema/user.py
from sqlalchemy.orm import Mapped, mapped_column, relationship
from typing import TYPE_CHECKING
from .base import Base

if TYPE_CHECKING:
    from .message import Message


class User(Base):
    __tablename__ = "user"

    id: Mapped[int] = mapped_column(primary_key=True, init=False)
    messages: Mapped[list["Message"]] = relationship(
        back_populates="user", default_factory=list
    )
```

but I had to change `message.py` like this:

```diff
--- a.py	2025-10-31 09:35:10
+++ src/app/db/schema/message.py	2025-10-31 09:39:14
@@ -13,8 +13,9 @@
 class Message(Base):
     __tablename__ = "message"
 
+    id: Mapped[UUID] = mapped_column(Uuid, primary_key=True)
     content: Mapped[str]
     user_id: Mapped[UUID] = mapped_column(Uuid, ForeignKey("user.id"))
 
     # This line
-    user: Mapped["User"] = relationship(back_populates="messages", init=False)
\ No newline at end of file
+    user: Mapped[User] = relationship(back_populates="messages")
```

This runs without error. Removing the quotes gives: 

```console
❯ echo 'from app.db.schema.message import Message' | uv run python3.14 -
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/dmbp/Documents/dev/app/src/app/db/schema/message.py", line 13, in <module>
    class Message(Base):
    ...<7 lines>...
        user: Mapped[User] = relationship(back_populates="messages")
  File "/Users/dmbp/Documents/dev/app/.venv/lib/python3.14/site-packages/sqlalchemy/orm/decl_api.py", line 626, in __init_subclass__
    super().__init_subclass__(**kw)
    ~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^
  File "/Users/dmbp/Documents/dev/app/.venv/lib/python3.14/site-packages/sqlalchemy/orm/decl_api.py", line 846, in __init_subclass__
    _as_declarative(cls._sa_registry, cls, cls.__dict__)
    ~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/dmbp/Documents/dev/app/.venv/lib/python3.14/site-packages/sqlalchemy/orm/decl_base.py", line 245, in _as_declarative
    return _MapperConfig.setup_mapping(registry, cls, dict_, None, {})
           ~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/dmbp/Documents/dev/app/.venv/lib/python3.14/site-packages/sqlalchemy/orm/decl_base.py", line 326, in setup_mapping
    return _ClassScanMapperConfig(
        registry, cls_, dict_, table, mapper_kw
    )
  File "/Users/dmbp/Documents/dev/app/.venv/lib/python3.14/site-packages/sqlalchemy/orm/decl_base.py", line 564, in __init__
    self._setup_dataclasses_transforms()
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^
  File "/Users/dmbp/Documents/dev/app/.venv/lib/python3.14/site-packages/sqlalchemy/orm/decl_base.py", line 1171, in _setup_dataclasses_transforms
    self._apply_dataclasses_to_any_class(
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^
        dataclass_setup_arguments, self.cls, annotations
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    )
    ^
  File "/Users/dmbp/Documents/dev/app/.venv/lib/python3.14/site-packages/sqlalchemy/orm/decl_base.py", line 1221, in _apply_dataclasses_to_any_class
    restored = getattr(klass, "__annotations__", None)
  File "/Users/dmbp/Documents/dev/app/src/app/db/schema/message.py", line 21, in __annotate__
    user: Mapped[User] = relationship(back_populates="messages")
                 ^^^^
NameError: name 'User' is not defined
```


---

_Comment by @Daverball on 2025-10-31 13:57_

@dylwil3 That does seem like a bug to me in SQLAlchemy's PEP-649 support, yes. It looks like they directly access `__annotations__` which causes `__annotate__` to execute with the default behavior, instead of in `FORWARDREF` mode, which is what I assume they actually would want to do here.

It looks like this specifically happens in the dataclass transforms path, does this also happen if you don't use `MappedAsDataclass`? Maybe they just haven't discovered and safe-guarded all the paths yet that directly access `__annotations__`. In previous versions accessing `__annotations__` can't fail, since it's compiled in, rather than computed and reifed at runtime

---

_Comment by @dylwil3 on 2025-10-31 14:42_

I had to tweak the example a bit to get it to run _with_ quotes, so @extrange I would still be interested in a repro that is specific to your case.

If I remove `MappedAsDataclass` then it runs with or without quotes. I started a [discussion](https://github.com/sqlalchemy/sqlalchemy/discussions/12951) upstream.

---

_Comment by @zzzeek on 2025-10-31 23:08_

> Maybe they just haven't discovered and safe-guarded all the paths yet that directly access `__annotations__`. 

that is what happened here, this was a case specific to dataclasses.   upstream issue is https://github.com/sqlalchemy/sqlalchemy/issues/12952 which is fixed for release 2.0.45



---

_Comment by @extrange on 2025-11-01 04:47_

> I had to tweak the example a bit to get it to run _with_ quotes, so [@extrange](https://github.com/extrange) I would still be interested in a repro that is specific to your case.
> 
> If I remove `MappedAsDataclass` then it runs with or without quotes. I started a [discussion](https://github.com/sqlalchemy/sqlalchemy/discussions/12951) upstream.

`example.py`:

```python
# /// script
# requires-python = "~=3.14.0"
# dependencies = [
#   "sqlalchemy==2.0.44"
# ]
# ///

from sqlalchemy import ForeignKey
from sqlalchemy.orm import (
    DeclarativeBase,
    Mapped,
    MappedAsDataclass,
    mapped_column,
    relationship,
)
from typing import TYPE_CHECKING


class Base(DeclarativeBase, MappedAsDataclass):
    id: Mapped[int] = mapped_column(primary_key=True)


if TYPE_CHECKING:

    class User(Base):
        __tablename__ = "user"


class Message(Base):
    __tablename__ = "message"

    user_id: Mapped[int] = mapped_column(ForeignKey("user.id"))
    user: Mapped["User"] = relationship(back_populates="messages", init=False)


print("ok")

```

```sh
❯ uv run example.py
ok
```

For Python 3.13, UP037 does not suggest unsafe fixes:

```sh
❯ echo $(ruff --version) $(python --version) && ruff check --select UP037 example.py 
ruff 0.14.3 Python 3.13.8
All checks passed!
```

However for 3.14:

```sh
❯ echo $(ruff --version) $(python --version) && ruff check --select UP037 example.py 
ruff 0.14.3 Python 3.14.0
UP037 [*] Remove quotes from type annotation
  --> example.py:30:18
   |
29 |     user_id: Mapped[int] = mapped_column(ForeignKey("user.id"))
30 |     user: Mapped["User"] = relationship(back_populates="messages", init=False)
   |                  ^^^^^^
   |
help: Remove quotes

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Applying the suggested fix gives:

```sh
❯ uv run example.py
# truncated
  File "example.py", line 33, in __annotate__
    user: Mapped[User] = relationship(back_populates="messages", init=False)
                 ^^^^
NameError: name 'User' is not defined
```

---

_Comment by @zzzeek on 2025-11-01 05:09_

i can't comment on what uv is doing however the current main/rel_2_0 branch of SQLAlchemy passes with the above test case as long as you run python 3.14.   on python 3.13, you get the NameError which is expected because the script lacks `from __future__ import annotations`.


```
[classic@framework sqlalchemy:rel_2_0]$ ~/.venv313/bin/python test3.py 
Traceback (most recent call last):
  File "/home/classic/dev/sqlalchemy/test3.py", line 29, in <module>
    class Message(Base):
    ...<3 lines>...
        user: Mapped[User] = relationship(back_populates="messages", init=False)
  File "/home/classic/dev/sqlalchemy/test3.py", line 33, in Message
    user: Mapped[User] = relationship(back_populates="messages", init=False)
                 ^^^^
NameError: name 'User' is not defined. Did you mean: 'user'?
[classic@framework sqlalchemy:rel_2_0]$ ~/.venv314/bin/python test3.py 
ok

```

---

_Comment by @dylwil3 on 2025-11-01 12:04_

Excellent! Thanks @extrange for the small repro, @Daverball for the suggestion about upstream, and especially thank you @zzzeek for the very quick fix! 

---

_Closed by @dylwil3 on 2025-11-01 12:05_

---
