```yaml
number: 1066
title: "import variable add an Unknown | to the type"
type: issue
state: closed
author: asukaminato0721
labels:
  - question
assignees: []
created_at: 2025-08-20T18:48:58Z
updated_at: 2025-10-06T16:41:11Z
url: https://github.com/astral-sh/ty/issues/1066
synced_at: 2026-01-12T15:54:24Z
```

# import variable add an Unknown | to the type

---

_@asukaminato0721_

### Summary

link https://github.com/langgenius/dify/blob/1caeac56f291a393fa2c418baa1e6c56d31f6655/api/commands.py#L94

in file a

```py
from flask_sqlalchemy import SQLAlchemy
db = SQLAlchemy(metadata=metadata)
```

in file b

```py
db.session.query(...)
```

db's type shows `Unknown | SQLAlchemy`, then all the chained methods become unknown.

<img width="726" height="123" alt="Image" src="https://github.com/user-attachments/assets/8cfea25e-429f-4d2c-a392-256f5e2f2db0" />


### Version

ty 0.0.1a19

---

_Comment by @sharkdp on 2025-08-20 18:59_

Thank you for reporting this. This behavior is intentional. You can read more about it here: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md.

You can get rid of the `Unknown` by annotating the type of `db` explicitly (e.g. `SQLAlchemy` or via `typing.Final`, if appropriate here).

---

_Label `question` added by @sharkdp on 2025-08-20 18:59_

---

_Comment by @carljm on 2025-08-20 22:04_

I've come to believe that we should not do this for un-annotated module globals: https://github.com/astral-sh/ty/issues/1069

---

_Comment by @vlashada on 2025-08-21 18:57_

I'm seeing this behavior when decorating a class with final. Is this expected behavior?

```python
@dataclasses.dataclass
class Person:
    name: str

@typing.final
class MyCollection:
    alice = Person(name="Alice")
    bob = Person(name="Bob")

MyCollection.alice # type: Unknown | Person
```

---

_Comment by @carljm on 2025-08-21 21:02_

The decoration with `typing.final` is unrelated there. The `Unknown | ` is because the `alice` and `bob` attributes are not annotated, so we don't know if you want to assign some other type to those attributes later. If you annotate the attributes with `: Person` (or with `: typing.Final`) the `Unknown |` will go away.

---

_Comment by @AlexWaygood on 2025-10-06 16:41_

Fixed in https://github.com/astral-sh/ruff/pull/20664

---

_Closed by @AlexWaygood on 2025-10-06 16:41_

---
