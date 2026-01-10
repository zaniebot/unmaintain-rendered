```yaml
number: 1231
title: ANN101 False Positive
type: issue
state: closed
author: Tomperez98
labels:
  - bug
assignees: []
created_at: 2022-12-13T21:42:58Z
updated_at: 2022-12-14T01:13:05Z
url: https://github.com/astral-sh/ruff/issues/1231
synced_at: 2026-01-10T12:06:19Z
```

# ANN101 False Positive

---

_Issue opened by @Tomperez98 on 2022-12-13 21:42_

```python
class AClass:
    """Loan Service exposes queries and commands required by the system."""

    def public_method(self, properties: Dict[Any, Any]) -> polars.DataFrame:
        """A Docstring."""
        _ = properties

```

```bash
ANN101 Missing type annotation for `self` in method
```

---

_Label `bug` added by @charliermarsh on 2022-12-13 23:22_

---

_Comment by @charliermarsh on 2022-12-13 23:23_

Weird, definitely a bug!

---

_Comment by @Tomperez98 on 2022-12-13 23:30_

If I write the code as

```python
class AClass:
    """Loan Service exposes queries and commands required by the system."""

    def public_method(self: "AClass", properties: Dict[Any, Any]) -> polars.DataFrame:
        """A Docstring."""
        _ = properties
```

It doesn't complain. Maybe that's the correct way to write it, but 💩  if it is

---

_Comment by @edgarrmondragon on 2022-12-13 23:40_

That's the behavior of [flake8-annotations](https://pypi.org/project/flake8-annotations/). The rationale is that inheritance patterns may depend on the concrete type of `self`.

```python
class A:
    def public_method(self: "A", properties: Dict[Any, Any]) -> polars.DataFrame:
        """A Docstring."""
        _ = properties


class B:
    new_attr = True

    def public_method(self: "A", properties: Dict[Any, Any]) -> polars.DataFrame:
        """A Docstring."""
        super().public_method(self, properties)
        self.new_attr  # mypy would complain that `A` doesn't have a `new_attr` attribute.
```

---

_Comment by @charliermarsh on 2022-12-13 23:40_

Wait, sorry, this actually _is_ intended, because there are special for `self` and `cls`.

---

_Comment by @charliermarsh on 2022-12-13 23:41_

It's best to disable `ANN101` and `ANN102` if you don't want to enforce those.

---

_Closed by @charliermarsh on 2022-12-14 00:13_

---

_Comment by @Tomperez98 on 2022-12-14 01:13_

Thanks @edgarrmondragon 

Btw, is amazing what you're doing with the Meltano project 

---
