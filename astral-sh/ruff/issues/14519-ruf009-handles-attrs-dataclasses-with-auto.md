```yaml
number: 14519
title: "`RUF009` handles `attrs` dataclasses with `auto_attribs = False` incorrectly"
type: issue
state: closed
author: InSyncWithFoo
labels: []
assignees: []
created_at: 2024-11-22T03:15:26Z
updated_at: 2024-11-24T02:46:39Z
url: https://github.com/astral-sh/ruff/issues/14519
synced_at: 2026-01-10T11:09:56Z
```

# `RUF009` handles `attrs` dataclasses with `auto_attribs = False` incorrectly

---

_Issue opened by @InSyncWithFoo on 2024-11-22 03:15_

From #14327:

> Note that `RUF009` can be a bit misleading when `auto_attribs=False` (or with older `attr.s` codebases):
> 
> ```python
> import attrs
> 
> @attrs.define(auto_attribs=False)
> class X:
>     a_field: int = attrs.field(default=42)
>     not_a_field: list = (lambda: [])()
> #                       ^^^^^^^^^^^^^^ RUF009 Do not perform function call in dataclass defaults
> ```
> 
> Without `auto_attribs`, `not_a_field` can't possibly be a `dataclass` field. In this example, it's an incorrectly typed class variable. Fixing the typing error also fixes the `RUF009` warning:
> 
> ```python
> @attrs.define(auto_attribs=False)
> class X:
>     a_field: int = attrs.field(default=42)
>     not_a_field: typing.ClassVar[list] = (lambda: [])()
> ```
> 
> This is an issue though when using `attrs.field` wrappers:
> 
> ```python
> def my_field():
>     return attrs.field(default=42, metadata={"my_metadata": 42})
> 
> 
> @attrs.define(auto_attribs=False)
> class X:
>     a_field: int = my_field()
> #                  ^^^^^^^^^^ RUF009 Do not perform function call `my_field` in dataclass defaults
> ```
>
> &mdash; @pwuertz



---

_Closed by @charliermarsh on 2024-11-24 02:46_

---

_Closed by @charliermarsh on 2024-11-24 02:46_

---
