```yaml
number: 384
title: Declared-only attributes are not accessible on instances of the class
type: issue
state: closed
author: rowillia
labels:
  - attribute access
  - metaprogramming
assignees: []
created_at: 2025-05-14T14:55:46Z
updated_at: 2025-07-02T16:03:57Z
url: https://github.com/astral-sh/ty/issues/384
synced_at: 2026-01-10T02:07:35Z
```

# Declared-only attributes are not accessible on instances of the class

---

_Issue opened by @rowillia on 2025-05-14 14:55_

### Summary

When using SQLAlchemy's declarative models with `Mapped` type annotations, ty incorrectly reports that model class attributes (which represent database columns) can only be accessed on instances, not on the class itself.

## Example

```python
from uuid import UUID
from sqlalchemy import select
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, Session


class Base(DeclarativeBase):
    pass


class Organization(Base):
    __tablename__ = "organizations"
    
    id: Mapped[int] = mapped_column(primary_key=True)
    uuid: Mapped[UUID] = mapped_column(unique=True)
    name: Mapped[str] = mapped_column()


def get_organization(session: Session, org_uuid: UUID):
    # ty incorrectly reports: "Attribute `uuid` can only be accessed on instances"
    return session.scalar(
        select(Organization).where(Organization.uuid == org_uuid)
    )
```

See SQLAlchemy's documentation on [querying](https://docs.sqlalchemy.org/en/20/tutorial/data_select.html) for more examples.


### Version

ty 0.0.1-alpha.1

---

_Label `bug` added by @sharkdp on 2025-05-14 14:58_

---

_Label `dataclasses` added by @sharkdp on 2025-05-14 14:58_

---

_Label `attribute access` added by @sharkdp on 2025-05-14 14:58_

---

_Comment by @sharkdp on 2025-05-14 15:08_

Thank you for reporting this.

I have trouble reproducing this:

```
> uv init
> uv add sqlalchemy
> uvx ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!
```

Can you please add more details how you ran `ty` and what the environment looks like?

I can see how we would show that message if there was no right hand side in the attribute assignment (`uuid: Mapped[UUID]`), but with the `mapped_column(…)` right-hand-side, we should understand that this can also be accessed on the class itself.

That said, we don't fully support dataclass fields yet, so I wouldn't expect support for sqlalchemy models to be complete (see #111)

---

_Comment by @rowillia on 2025-05-14 15:59_

@sharkdp sorry about that!  I made a mistake when trying to create a minimal repro 😄 - does this work better?

```python
from sqlalchemy import select
from sqlalchemy.orm import DeclarativeBase, Mapped, Session


class Base(DeclarativeBase):
    pass


class User(Base):
    __tablename__ = "users"

    id: Mapped[int]  # Primary key column
    name: Mapped[str]  # String column
    email: Mapped[str]  # Another string column


def get_user_by_id(session: Session, user_id: int):
    return session.scalar(select(User).where(User.id == user_id))


def search_users_by_name(session: Session, name_pattern: str):
    return session.scalars(
        select(User).where(User.name.ilike(f"%{name_pattern}%"))
    ).all()
```

```bash
$ ty check --python $(which python) /tmp/sqlalchemy_mapped_minimal.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Attribute `id` can only be accessed on instances, not on the class object `<class 'User'>` itself.
  --> /tmp/sqlalchemy_mapped_minimal.py:18:46
   |
17 | def get_user_by_id(session: Session, user_id: int):
18 |     return session.scalar(select(User).where(User.id == user_id))
   |                                              ^^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Attribute `name` can only be accessed on instances, not on the class object `<class 'User'>` itself.
  --> /tmp/sqlalchemy_mapped_minimal.py:28:28
   |
26 |     """
27 |     return session.scalars(
28 |         select(User).where(User.name.ilike(f"%{name_pattern}%"))
   |                            ^^^^^^^^^
29 |     ).all()
   |
info: rule `unresolved-attribute` is enabled by default

Found 2 diagnostics
```

---

_Comment by @carljm on 2025-05-14 17:55_

Presumably there is some runtime magic in `DeclarativeBase` (a metaclass or `__init_subclass__` or something) which places an actual attribute on the class, based on those annotations? It feels unfortunate to have to let this pass silently in the vast majority of cases where the attribute wouldn't be available on the class (since, obviously, nothing is assigned on the class) in order to support this.

Does it break the typing of this in some way to wrap those annotations in `typing.ClassVar`?

---

_Comment by @carljm on 2025-05-15 02:49_

Wrapping the three `Mapped` annotations in `ClassVar` does make ty happy, without upsetting either mypy or pyright. I feel like that's the better annotation for this case, since it explicitly asserts "this attribute is accessible on the class, even though it would apparently not seem to be."

---

_Comment by @shaunhegarty on 2025-05-15 11:49_

I'm seeing this issue as well. Adding ClassVar to every single attribute like this does indeed make ty happy, but will make unhappy developers because we would need to apply this to many attributes.

---

_Comment by @carljm on 2025-05-17 21:41_

Yeah, understood that annotating all such attributes with `ClassVar` may not be appealing. I guess we could special-case some knowledge of SQLAlchemy's DeclarativeBase? Seems like our options are basically that, or relaxing this rule entirely.

---

_Comment by @AlexWaygood on 2025-05-17 21:46_

> I guess we could special-case some knowledge of SQLAlchemy's DeclarativeBase?

I don't love the idea that these classes will have different semantics to all other possible classes, but this feels like our best option right now. It doesn't close the door to relaxing the rule entirely in the future if we find other common cases where this rule causes problems.

---

_Comment by @JaRoSchm on 2025-05-27 07:51_

Something similar happens for mongoengine which is an ORM for MongoDB. I tried to copy the relevant parts from the mongoengine code to get a minimal example: https://play.ty.dev/b625af2d-5c85-40b5-841e-c8ff6b14c4ba
I cannot say that I really understand this code but indeed it seems like the attribute is added by a metaclass which is not detected by ty. If you think this is unrelated I can open a separate issue.

---

_Comment by @sharkdp on 2025-05-28 07:50_

> I cannot say that I really understand this code but indeed it seems like the attribute is added by a metaclass which is not detected by ty. If you think this is unrelated I can open a separate issue.

I think this is slightly different. In this case, `Test` has no annotation at all. The `objects` attribute seems to be something that is implicitly available on classes that subclass `Document`. So it seems to me like mongoengine could add a `objects: ClassVar[list[object]]` annotation to `TopLevelDocumentMetaclass` to make this explicit: https://play.ty.dev/224a3bdc-54c1-4fbf-8991-723aa526c002.

---

_Label `dataclasses` removed by @AlexWaygood on 2025-05-28 15:38_

---

_Label `metaprogramming` added by @AlexWaygood on 2025-05-28 15:38_

---

_Label `bug` removed by @AlexWaygood on 2025-05-28 15:38_

---

_Comment by @layday on 2025-05-29 21:00_

Wrapping a descriptor with a class-bound `__get__` overload in `ClassVar` seems redundant.  Perhaps ty should interpret these descriptors as having an implicit `ClassVar` component. `ClassVar`-less descriptors also seem to misbehave, see lines 23, 38 and 39 here: https://play.ty.dev/9c511571-d065-4df8-af07-2e7d62058918.

---

_Comment by @carljm on 2025-05-29 21:18_

> Perhaps ty should interpret these descriptors as having an implicit `ClassVar` component.

What would define the category of "these descriptors"?

> `ClassVar`-less descriptors also seem to misbehave, see lines 23, 38 and 39 here: https://play.ty.dev/9c511571-d065-4df8-af07-2e7d62058918.

I think all of the behavior there is intended.

If you annotate an attribute on a class, not as a `ClassVar`, and don't assign anything to it, we take that as implying "this is an attribute present on instances, not on the class." This is reasonable IMO, since it's not a `ClassVar` and nothing was assigned on the class -- but it's also causing the difficulty in this issue (where a magic metaclass is actually assigning something on the class behind the scenes.) If a descriptor is present on an instance but not on the class, the descriptor protocol doesn't come into play: that explains the result on line 23.

The `Unknown |` component of the type on lines 38 and 39 is because there is no annotation, and we don't assume that an unannotated class attribute is forever tied to its initially assigned type. E.g. we don't assume that in

```py
class RetryPolicy:
    max_retries = None
```

that the (unannotated) `max_retries` should be assumed to be restricted to always be `None`. So we infer a type of `Unknown | None` for it, since it is not annotated with any declared type restricting future reassignments.

---

_Comment by @carljm on 2025-05-30 00:24_

https://github.com/astral-sh/ty/issues/539 has some more ways this can manifest.

---

_Comment by @layday on 2025-05-30 08:25_

> What would define the category of "these descriptors"?

A `__get__` overload where the instance is of type `None` can only logically apply to the class.  It’s not clean, but it might be better than carving out an exception for SQLAlchemy specifically?

Thank you for explaining ty’s approach to class variables.

---

_Comment by @carljm on 2025-05-30 20:26_

After seeing another non-SQLAlchemy case of this in https://github.com/astral-sh/ty/issues/553, I'm leaning towards just abandoning our current strict handling of annotated-but-not-assigned names in class bodies, and just allow them to be considered a class-or-instance attribute, as mypy and pyright do. I still think it would have been better for the type system to go a different direction here, but I don't want to take on the burden of incompatibility with too much existing typed code.

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:44_

---

_Comment by @fightingdreamer on 2025-06-24 21:41_

Can `ty` have strict mode for cases like this?

Cause keeping compatibility with pyright, basedpyright and pyrefly can ease transition, at the cost of introducing non optimal behavior later on — assuming the end goal is to move to `ty` completely.

---

_Comment by @carljm on 2025-06-24 22:16_

> and pyrefly

This surprised me a bit, since last I knew, pyrefly implemented the same thing we do, that `x: int` with no assignment and no `ClassVar` in a class body means "instance-only attribute". And [that does seem to still be the case](https://pyrefly.org/sandbox/?code=MYGwhgzhAEDCBcAoaLoA97QJYDsAuiisAdGoUA).

But pyrefly doesn't error on this SQLAlchemy example. As best I can tell, the reason is because they implement a heuristic that if the annotated type of the attribute appears to be a descriptor, they then [assume that it is actually accessible on the class](https://pyrefly.org/sandbox/?code=MYGwhgzhAEAiBcAoaLoBMCmAzaB9XA5hgC74AUEGIWANNAJYB2ExYjwGdA9gO6MYAnAJRJUY6AJIBXAY2gA5Lv0SJQkGAGFRqAB7wGjYslRp9sFRoB0OxFbRA).

So that's another option here, but I don't think it's a good option. I don't think the details of the annotated type should impact whether we consider something to be an instance-only attribute or a class-and-instance-attribute. I would rather just follow pyright and mypy in assuming that everything annotated on the class can be accessed on the class.

---

_Comment by @AlexWaygood on 2025-06-24 22:18_

> So that's another option here, but I don't think it's a good option. I don't think the details of the annotated type should impact whether we consider something to be an instance-only attribute or a class-and-instance-attribute. I would rather just follow pyright and mypy in assuming that everything annotated on the class can be accessed on the class.

I do like the idea of having a disabled-by-default rule that flags places our current behaviour would flag, so that those who do want additional type safety can have it. But yes, I agree that we will probably need to copy mypy/pyright for our default behaviour here. The incompatibility with existing type annotations will probably be too great otherwise :-(

---

_Renamed from "SQLAlchemy Model Class Attribute Access Incorrectly Flagged as Error" to "Declared-only attributes are not accessible on instances of the class" by @sharkdp on 2025-06-26 14:35_

---

_Closed by @sharkdp on 2025-07-02 16:03_

---
