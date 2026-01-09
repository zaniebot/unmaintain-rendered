---
number: 13969
title: "[django] New rule: Check that enums do not have a `do_not_call_in_templates` member"
type: issue
state: open
author: Kakadus
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-10-28T21:17:24Z
updated_at: 2024-10-30T07:51:56Z
url: https://github.com/astral-sh/ruff/issues/13969
synced_at: 2026-01-07T13:12:16-06:00
---

# [django] New rule: Check that enums do not have a `do_not_call_in_templates` member

---

_Issue opened by @Kakadus on 2024-10-28 21:17_

In django templates, if a template variable is callable, the template system will try calling it. To disable that, one can define a truthy `do_not_call_in_templates` attribute on the callable [[ref. django docs]](https://docs.djangoproject.com/en/5.1/ref/templates/api/#variables-and-lookups). 

A fallacy might be to do something like the following:

```py
from enum import Enum, auto


class AwesomeEnum(Enum):
    do_not_call_in_templates = True

    CASE_A = auto()
    CASE_B = auto()
```

Note that that leads `do_not_call_in_templates` to be e.g. in `__members__`, and maps to `AwesomeEnum(True)`:
```py
>>> AwesomeEnum.__members__
{'do_not_call_in_templates': <AwesomeEnum.do_not_call_in_templates: True>, 'CASE_A': <AwesomeEnum.CASE_A: 2>, 'CASE_B': <AwesomeEnum.CASE_B: 3>}
```

Instead, it should either be wrapped in an `enum.nonmember` or defined in a `@property`, so that it is not exported as a member of the enum [like django itself does it](https://github.com/django/django/blob/555f2412cba4c5844408042e92f3bf9fa5c2392c/django/db/models/enums.py#L81-L90).

A correct Enum would be defined like so:

```py
from enum import Enum, auto, nonmember


class AwesomeEnum(Enum):
    do_not_call_in_templates = nonmember(True)

    CASE_A = auto()
    CASE_B = auto()
```
or (as there is no `nonmember` in <3.11):
```py
class AwesomeEnum(Enum):
    @property
    def do_not_call_in_templates(self):
        return True

    CASE_A = auto()
    CASE_B = auto()
```

Maybe this is can be implemented as a nice little ruff rule? We just noticed this after having a `do_not_call_in_templates` member in our enum for nearly 4 years :smile: 

---

_Label `rule` added by @MichaReiser on 2024-10-30 07:51_

---

_Comment by @MichaReiser on 2024-10-30 07:51_

Thanks for the nice write up. This does make sense to me. The only decision we have to make here is to which extend we want to have django rules in general. 

---

_Label `needs-decision` added by @MichaReiser on 2024-10-30 07:51_

---
