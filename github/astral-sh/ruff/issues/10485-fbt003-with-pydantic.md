---
number: 10485
title: "`FBT003` with Pydantic"
type: issue
state: closed
author: redb0
labels:
  - rule
  - configuration
assignees: []
created_at: 2024-03-20T04:45:06Z
updated_at: 2024-03-30T00:26:14Z
url: https://github.com/astral-sh/ruff/issues/10485
synced_at: 2026-01-07T13:12:15-06:00
---

# `FBT003` with Pydantic

---

_Issue opened by @redb0 on 2024-03-20 04:45_

Hello!

I use Ruff version `0.1.15` with `flake8-boolean-trap`. I am getting this error in the following example:

```python
from pydantic import Field
from pydantic_settings import BaseSettings


class Settings(BaseSettings):

    foo: bool = Field(True, exclude=True)
```

Command `ruff test.py --fix` return

```
test.py:7:23: FBT003 Boolean positional value in function call
```

Pydantic allows you to explicitly specify the `default` parameter (`foo: bool = Field(default=True, ...)`). However, this stands out from the general code, where other values ​​are specified without it.

I’m not sure that it’s worth “strictly” filtering such cases. On the one hand, I can disable this check for certain files. I think it's not right to do this. Maybe it’s worth allowing the user to independently specify filtering rules, in addition to those initially specified as here https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_boolean_trap/helpers.rs#L7?

I'd be glad to hear your opinion.




---

_Label `rule` added by @dhruvmanila on 2024-03-20 07:19_

---

_Label `needs-decision` added by @dhruvmanila on 2024-03-20 07:19_

---

_Comment by @zanieb on 2024-03-20 14:21_

Basically the same concept as https://github.com/astral-sh/ruff/issues/10356#issuecomment-1991780371

I think we should add a configurable list of allowed types / calls.

---

_Label `needs-decision` removed by @zanieb on 2024-03-20 14:21_

---

_Label `configuration` added by @zanieb on 2024-03-20 14:21_

---

_Comment by @redb0 on 2024-03-20 14:35_

@zanieb Yes, I also encountered several similar problems #4172, #4382, #6711 and other. I think a list of available names/calls would be the most flexible solution

---

_Comment by @augustelalande on 2024-03-22 21:37_

I will implement this

---

_Referenced in [astral-sh/ruff#10531](../../astral-sh/ruff/pulls/10531.md) on 2024-03-23 00:27_

---

_Closed by @charliermarsh on 2024-03-30 00:26_

---
