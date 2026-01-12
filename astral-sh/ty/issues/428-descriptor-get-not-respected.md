```yaml
number: 428
title: "Descriptor / `__get__` not respected"
type: issue
state: closed
author: kreathon
labels:
  - question
  - attribute access
assignees: []
created_at: 2025-05-16T18:49:50Z
updated_at: 2025-05-17T19:04:25Z
url: https://github.com/astral-sh/ty/issues/428
synced_at: 2026-01-12T15:54:23Z
```

# Descriptor / `__get__` not respected

---

_@kreathon_

### Summary

In the following example, it seems that the `__get__` method of the descriptor is not respected. 

```python
from typing import Any
from typing import reveal_type


class Descriptor[T]:

    def __get__(self, instance: object, owner: Any) -> T: ...


class A:
    a: Descriptor[int]
    

reveal_type(A().a)
```

Output:
```
Descriptor[int]
```

Expected Output:
```
int
```

 

### Version

ty 0.0.1-alpha.4

---

_Comment by @sharkdp on 2025-05-16 21:13_

Thank you for reporting this.

`A.a` is only declared, not defined. We treat this as a hint that the `a` attribute is a pure instance attribute, which is not  accessible on the class itself. And instance attributes do not invoke the descriptor protocol. Try defining `A.a` and it should work.

We're also interested in feedback on this design decision.

---

_Label `question` added by @sharkdp on 2025-05-17 06:52_

---

_Label `attribute access` added by @sharkdp on 2025-05-17 06:52_

---

_Comment by @kreathon on 2025-05-17 18:59_

I came up with the example from above while playing around with `ty` and a codebase using `sqlalchemy`. 

Here is a simple model:

```python
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import DeclaritiveBase
from sqlalchemy.orm import mapped_column


class Base(DeclaritiveBase):
    pass


class User(Base):
    __tablename__ = "user"

    id: Mapped[int] = mapped_column(primary_key=True)

    name: Mapped[str]
```

Attributes like `name` can either be used to access the instance value or at the class level as part of a query:

```python

user: User = session.execute(
    sa.select(User)
    .filter(User.name == "ty")
).scalar_one()

print(user.name)
```

This code can currently not be type checked with `ty`:

```python
user.name   # Mapped[str]
User.name   # Attribute `name` can only be accessed on instances, not on the class object `<class 'User'`>` itself.

```

[Definition](https://github.com/sqlalchemy/sqlalchemy/blob/main/lib/sqlalchemy/orm/base.py#L763) of `Mapped`:

```python
class Mapped(
    SQLORMExpression[_T_co],
    ORMDescriptor[_T_co],
    _MappedAnnotationBase[_T_co],
    roles.DDLConstraintColumnRole,
):
     ...

    if typing.TYPE_CHECKING:

        @overload
        def __get__(
            self, instance: None, owner: Any
        ) -> InstrumentedAttribute[_T_co]: ...

        @overload
        def __get__(self, instance: object, owner: Any) -> _T_co: ...

        def __get__(
            self, instance: Optional[object], owner: Any
        ) -> Union[InstrumentedAttribute[_T_co], _T_co]: ...

    ...
```

The problem could be solved (as described by @sharkdp ) by adding a definition:

```python
name: Mapped[str] = mapped_column()
```

Is there anything that could be done on the `sqlalchemy` end?

Do we need to add the `mapped_column` assignments (that are otherwise useless and make the code less readable)?



---

_Comment by @sharkdp on 2025-05-17 19:04_

I see, thank you for providing that background. I'll close this ticket in favor of https://github.com/astral-sh/ty/issues/384, where the same question is being discussed (see also the `ClassVar` suggestion there).

---

_Closed by @sharkdp on 2025-05-17 19:04_

---
