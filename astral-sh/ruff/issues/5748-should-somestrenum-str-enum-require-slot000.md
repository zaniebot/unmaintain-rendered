```yaml
number: 5748
title: "Should `SomeStrEnum(str, Enum)` require `SLOT000`?"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-07-13T19:42:12Z
updated_at: 2023-08-11T13:58:07Z
url: https://github.com/astral-sh/ruff/issues/5748
synced_at: 2026-01-12T15:54:45Z
```

# Should `SomeStrEnum(str, Enum)` require `SLOT000`?

---

_@charliermarsh_

### Discussed in https://github.com/astral-sh/ruff/discussions/5747

<div type='discussions-op-text'>

<sup>Originally posted by **jamesbraza** July 13, 2023</sup>
Please see the below file with Python 3.10.10:

```python
from enum import Enum

class SomeStrEnum(str, Enum):
    KEY = "value"
```

Running `ruff==0.0.278` on this:

```bash
> ruff --isolated --select="SLOT" a.py
a.py:3:7: SLOT000 Subclasses of `str` should define `__slots__`
Found 1 error.
```

Running `flake8-slots==0.1.5` with `flake8==4.0.1` on this, there's no error:

```bash
> flake8 a.py
```

Fwiw, Python 3.11's built-in [`enum.StrEnum`](https://docs.python.org/3/library/enum.html#enum.StrEnum) doesn't seem to define `__slots__`.

I am not sure if there should or should not be `SLOT000` for this.  Looks like @qdegraaf added `SLOT` rule in `ruff==0.0.273`, so notifying him.

I am thinking either:
- This is a false positive of `SLOT000`
- Or should the built-in `StrEnum` define `__slots__`? </div>

---

_Comment by @charliermarsh on 2023-07-13 19:42_

Looks like `flake8-slots` omits enums: https://github.com/python-formate/flake8-slots/blob/0b1ecb9076c6194f29265b7bfb904d63073bce08/flake8_slots/__init__.py#L102.

---

_Label `bug` added by @charliermarsh on 2023-07-13 19:43_

---

_Label `accepted` added by @charliermarsh on 2023-07-13 19:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-13 19:54_

---

_Closed by @charliermarsh on 2023-07-13 20:12_

---

_Comment by @sujuka99 on 2023-08-11 13:45_

Is this expected?

```py
class SuperEnum(Enum):
	...

class CustomStrEnum(str, SuperEnum):  # Results in "SLOT000 Subclasses of `str` should define `__slots__`"
	...
```

---

_Comment by @charliermarsh on 2023-08-11 13:49_

Yeah, we don't trace across base classes right now. You'd need to add `Enum` as a parent class to have it detected properly.

---

_Comment by @sujuka99 on 2023-08-11 13:58_

> Yeah, we don't trace across base classes right now. You'd need to add `Enum` as a parent class to have it detected properly.

Thanks for the quick answer :)

---
