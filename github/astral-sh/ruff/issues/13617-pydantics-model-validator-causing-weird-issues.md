---
number: 13617
title: Pydantics model validator causing weird issues when in classmethod-decorators
type: issue
state: closed
author: ollz272
labels:
  - question
  - needs-info
assignees: []
created_at: 2024-10-03T17:55:32Z
updated_at: 2024-11-20T12:04:07Z
url: https://github.com/astral-sh/ruff/issues/13617
synced_at: 2026-01-07T13:12:15-06:00
---

# Pydantics model validator causing weird issues when in classmethod-decorators

---

_Issue opened by @ollz272 on 2024-10-03 17:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

We have the following in our ruff config:

```.toml
[tool.ruff.lint.pep8-naming]
classmethod-decorators = ["classmethod", "pydantic.model_validator", "pydantic.field_validator"]
```

This is so we can do the following in our code:
```python
import pydantic
from pydantic import BaseModel

class Model(BaseModel):
    """Some Model."""

    foo: str
    bar: int

    @pydantic.model_validator(mode="before")
    def some_validation(cls, data):
        """Before Validation."""
        if data["foo"] == "hello" and data["bar"] == 1:
            raise ValueError

        return data
```

This works well, however when we set the validator mode to "after", this is an instance method rather than a class method, so we get:
```python
import pydantic
from pydantic import BaseModel


class Model(BaseModel):
    """Some Model."""

    foo: str
    bar: int

    @pydantic.model_validator(mode="before")
    def before_validation(cls, data):
        """Before Validation."""
        if data["foo"] == "hello" and data["bar"] == 1:
            raise ValueError

        return data

    @pydantic.model_validator(mode="after")
    def after_validation(self):
        """After Validation."""
        if not self.bar:
            raise ValueError

        return self

```

We get the following:
```
 N804 First argument of a class method should be named `cls`
   |
19 |     @pydantic.model_validator(mode="after")
20 |     def after_validation(self):
   |                          ^^^^ N804
21 |         """After Validation."""
22 |         if not self.bar:
   |
   = help: Rename `self` to `cls`
```

Which is slightly... annoying. We know we can remove this from config and add a class method decorator, but this has always felt a bit verbose to use.

Is there a better way to handle this case? is this something ruff would consider fixing?

---

_Comment by @zanieb on 2024-10-03 19:44_

How would you imagine us fixing this? Taking the full invocation in the config?

```toml
classmethod-decorators = ["classmethod", "pydantic.model_validator(mode=\"before\")", "pydantic.field_validator"]
```

---

_Comment by @dhruvmanila on 2024-10-04 04:15_

FWIW, the Pydantic docs do recommend to add the `@classmethod` decorator:

> The first argument should be `cls` (and we also recommend you use `@classmethod` below `@model_validator` for proper type checking), [...]
>
> https://docs.pydantic.dev/latest/concepts/validators/#field-validators

And, similarly for field validators:

> A few things to note on validators:
>
> 1. `@field_validators` are "class methods", so the first argument value they receive is the `UserModel` class, not an instance of `UserModel`. We recommend you use the `@classmethod` decorator on them below the `@field_validator` decorator to get proper type checking.
>
> https://docs.pydantic.dev/latest/concepts/validators/#field-validators

Do you use any type checker like mypy or Pyright? Do they work properly?

---

_Label `question` added by @MichaReiser on 2024-10-07 08:08_

---

_Label `needs-info` added by @MichaReiser on 2024-10-07 08:08_

---

_Closed by @MichaReiser on 2024-11-20 12:04_

---
