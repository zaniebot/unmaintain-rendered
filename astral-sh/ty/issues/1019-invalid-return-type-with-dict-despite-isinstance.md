```yaml
number: 1019
title: invalid-return-type with dict despite isinstance checks
type: issue
state: closed
author: CharlesPerrotMinot
labels:
  - question
assignees: []
created_at: 2025-08-16T05:47:31Z
updated_at: 2025-08-16T14:54:58Z
url: https://github.com/astral-sh/ty/issues/1019
synced_at: 2026-01-12T15:54:24Z
```

# invalid-return-type with dict despite isinstance checks

---

_@CharlesPerrotMinot_

### Question

Hi,

I'm wondering if this isn't an issue:
```
from typing import Self


class Test:
    @classmethod
    def validate_data(cls, data: str | dict[str, str] | Self) -> Self:
        if isinstance(data, str):
            return cls.from_json(data)
        if isinstance(data, dict):
            return cls(**data)
        return data
```

Gives the following error
```
error[invalid-return-type]: Return type does not match returned value
  --> test.py:6:62
   |
 4 | class Test:
 5 |     @classmethod
 6 |     def validate_data(cls, data: str | dict[str, str] | Self) -> Self:
   |                                                              ---- Expected `Self@validate_data` because of return type
 7 |         if isinstance(data, str):
 8 |             return cls.from_json(data)
 9 |         if isinstance(data, dict):
10 |             return cls(**data)
11 |         return data
   |                ^^^^ expected `Self@validate_data`, found `(dict[str, str] & ~dict[Unknown, Unknown]) | (Self@validate_data & ~str & ~dict[Unknown, Unknown])`
   |
```

Except... since I'm removing the possibility where data is either a str or a dict, it can only be a Self, hence line 11 can only return when it's of type Self. Or am I missing something?

### Version

[0.0.1-alpha.18](https://github.com/astral-sh/ty/releases/tag/0.0.1-alpha.18)

---

_Label `question` added by @CharlesPerrotMinot on 2025-08-16 05:47_

---

_Renamed from "invalid-return-type" to "invalid-return-type with dict despite isinstance checks" by @CharlesPerrotMinot on 2025-08-16 05:48_

---

_Comment by @CharlesPerrotMinot on 2025-08-16 05:54_

One workaround I found is to change the return value to the quote name of the class, check for type, and error if none of the expected one are received
```
from typing import Self


class Test:
    @classmethod
    def validate_data(cls, data: str | dict | Self) -> "Test":
        if isinstance(data, cls):
            return data
        if isinstance(data, str):
            return cls.from_json(data)
        if isinstance(data, dict):
            return cls(**data)
        raise TypeError("not a supported input")
```

---

_Comment by @carljm on 2025-08-16 14:54_

Thanks for the report! Yes, this is a known issue in our type narrowing for instances of generic classes: #456 

---

_Closed by @carljm on 2025-08-16 14:54_

---
