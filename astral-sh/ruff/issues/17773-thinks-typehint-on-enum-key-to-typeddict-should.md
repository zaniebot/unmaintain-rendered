```yaml
number: 17773
title: thinks typehint on enum key to TypedDict should be an assignment
type: issue
state: closed
author: theavey
labels:
  - needs-info
assignees: []
created_at: 2025-05-01T17:44:39Z
updated_at: 2025-05-05T15:09:52Z
url: https://github.com/astral-sh/ruff/issues/17773
synced_at: 2026-01-10T11:09:58Z
```

# thinks typehint on enum key to TypedDict should be an assignment

---

_Issue opened by @theavey on 2025-05-01 17:44_

### Summary

The following code (incorrectly, AFAIK) is flagged by rule B032
```python
import datetime
from enum import StrEnum
from typing import TypedDict


class RuleEnum(StrEnum):
    DATE = "date"
    SIGNAL = "signal"


class RuleType(TypedDict):
    RuleEnum.DATE: datetime.date
    date: datetime.date
```

```
scratch_19.py:12:5: B032 Possible unintentional type annotation (using `:`). Did you mean to assign (using `=`)?
   |
11 | class RuleType(TypedDict):
12 |     RuleEnum.DATE: datetime.date
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ B032
13 |     date: datetime.date
   |

Found 1 error.
```

Enums are valid (Typed)Dict keys, and still need to be annotated for a TypedDict

### Version

ruff 0.11.7

---

_Comment by @MichaReiser on 2025-05-01 18:23_

Neither [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=a0a295b6ab54def0f421da1b73a32d5a) nor [pyright](https://pyright-play.net/?pythonVersion=3.8&strict=true&enableExperimentalFeatures=true&reportImplicitOverride=true&reportShadowedImports=true&reportUnusedCallResult=true&code=JYWwDg9gTgLgBAEwIYwKY1KgUAMyhEOVAOwFdDRJY4BlGKAUTJF30JgE8xhiBzOStHgAVLqgQARYAGMYWedIA2SAM4q4AJVKLUTcgAo6jZgEoAXFjhW4EgILCGcALxwARMjSvL1mgEkA4gBytgAyzm4qwLzESIpeCspqmtqoomCo%2BmniUrLm3lZaOnogAHR2DmaIKOiYJR7Y1jb2DJX1GCCoddVAA) like this annotation.

Can you tell me more why you think this annotation should be allowed

---

_Label `needs-info` added by @MichaReiser on 2025-05-01 18:24_

---

_Comment by @theavey on 2025-05-05 15:09_

The following is effectively the same, but does not get flagged

```python
import datetime
from enum import StrEnum
from typing import TypedDict


class RuleEnum(StrEnum):
    DATE = "date"
    SIGNAL = "signal"

DATE = RuleEnum.DATE

class RuleType(TypedDict):
    DATE: datetime.date
    date: datetime.date
```
(note, changed the case of the second key of the typed dict to lower case to differentiate from the enum key)

It seems that mypy and pyright also both like this syntax better.

It is a personal preference, but I tend to access/use enum members as attributes of the enum, to keep it very clear they are instances of an enum. For example, I would write
```python
def func(input_enum: RuleEnum):
    if input_enum is RuleEnum.DATE:
        process_date_rule()
    elif input_enum is RuleEnum.SIGNAL:
        process_signal_rule()
```

Okay, so now that I look at this some more, the attributes of the TypedDict declaration are expected to be string keys (this is [explicitly stated in the PEP](https://peps.python.org/pep-0589/#specification)), so I'm just using it wrong.

Thank you

---

_Closed by @theavey on 2025-05-05 15:09_

---
