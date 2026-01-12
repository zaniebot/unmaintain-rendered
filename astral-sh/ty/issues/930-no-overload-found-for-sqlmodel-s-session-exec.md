```yaml
number: 930
title: "No overload found for SQLModel's `session.exec`"
type: issue
state: closed
author: aclementev
labels: []
assignees: []
created_at: 2025-08-03T11:13:12Z
updated_at: 2025-08-08T08:54:37Z
url: https://github.com/astral-sh/ty/issues/930
synced_at: 2026-01-12T15:54:24Z
```

# No overload found for SQLModel's `session.exec`

---

_@aclementev_

### Summary

I ran into the following error while running the [SQLModel tutorial](http://sqlmodel.tiangolo.com/tutorial/) using `ty`. 
```
No overload of bound method `exec` matches arguments
```

This is when running the following:

```python
    with Session(engine) as session:
        session.exec(delete(Hero))
```

Here's a full repro script: 

```python
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "sqlmodel",
# ]
# ///
from sqlmodel import (
    Field,
    Session,
    SQLModel,
    create_engine,
    delete,
)


class Hero(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str


def main():
    engine = create_engine("sqlite://", echo=True)
    SQLModel.metadata.create_all(engine)

    with Session(engine) as session:
        session.exec(delete(Hero))


if __name__ == "__main__":
    main()
``` 

When I run `ty check repro.py` in an venv with `sqlmodel==0.0.24` I see the following: 


```
error[no-matching-overload]: No overload of bound method `exec` matches arguments
  --> repro.py:26:9
   |
25 |     with Session(engine) as session:
26 |         session.exec(delete(Hero))
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
info: First overload defined here
  --> .venv/lib/python3.13/site-packages/sqlmodel/orm/session.py:29:9
   |
27 |   class Session(_Session):
28 |       @overload
29 |       def exec(
   |  _________^
30 | |         self,
31 | |         statement: Select[_TSelectParam],
32 | |         *,
33 | |         params: Optional[Union[Mapping[str, Any], Sequence[Mapping[str, Any]]]] = None,
34 | |         execution_options: Mapping[str, Any] = util.EMPTY_DICT,
35 | |         bind_arguments: Optional[Dict[str, Any]] = None,
36 | |         _parent_execute_state: Optional[Any] = None,
37 | |         _add_event: Optional[Any] = None,
38 | |     ) -> TupleResult[_TSelectParam]: ...
   | |___________________________________^
39 |
40 |       @overload
   |
info: Possible overloads for bound method `exec`:
info:   (self, statement: Select[_TSelectParam], *, params: Mapping[str, Any] | Sequence[Mapping[str, Any]] | None = None, execution_options: Mapping[str, Any] = @Todo, bind_arguments: dict[str, Any] | None = None, _parent_execute_state: Any | None = None, _add_event: Any | None = None) -> TupleResult[_TSelectParam]
info:   (self, statement: SelectOfScalar[_TSelectParam], *, params: Mapping[str, Any] | Sequence[Mapping[str, Any]] | None = None, execution_options: Mapping[str, Any] = @Todo, bind_arguments: dict[str, Any] | None = None, _parent_execute_state: Any | None = None, _add_event: Any | None = None) -> ScalarResult[_TSelectParam]
info: Overload implementation defined here
  --> .venv/lib/python3.13/site-packages/sqlmodel/orm/session.py:52:9
   |
50 |       ) -> ScalarResult[_TSelectParam]: ...
51 |
52 |       def exec(
   |  _________^
53 | |         self,
54 | |         statement: Union[
55 | |             Select[_TSelectParam],
56 | |             SelectOfScalar[_TSelectParam],
57 | |             Executable[_TSelectParam],
58 | |         ],
59 | |         *,
60 | |         params: Optional[Union[Mapping[str, Any], Sequence[Mapping[str, Any]]]] = None,
61 | |         execution_options: Mapping[str, Any] = util.EMPTY_DICT,
62 | |         bind_arguments: Optional[Dict[str, Any]] = None,
63 | |         _parent_execute_state: Optional[Any] = None,
64 | |         _add_event: Optional[Any] = None,
65 | |     ) -> Union[TupleResult[_TSelectParam], ScalarResult[_TSelectParam]]:
   | |_______________________________________________________________________^
66 |           results = super().execute(
67 |               statement,
   |
info: rule `no-matching-overload` is enabled by default

Found 1 diagnostic
``` 

When I run this with mypy this does not raise any error

### Version

ty 0.0.1-alpha.16 (c452e53a0 2025-07-25)

---

_Comment by @aclementev on 2025-08-03 19:44_

Never mind, I thought I checked it with mypy too but apparently I didn't. I ran mypy again and I see a similar error. 
This seems to be an issue with `sqlmodel`'s types (https://github.com/fastapi/sqlmodel/issues/909). 
Sorry for the noise

---

_Closed by @aclementev on 2025-08-03 19:44_

---

_Comment by @dhruvmanila on 2025-08-08 08:54_

No problem, thank you for following through and providing all the information!

---
