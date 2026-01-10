```yaml
number: 15132
title: "`PIE796` false-positives on enum member cast in pyi files"
type: issue
state: closed
author: Avasam
labels:
  - bug
  - rule
assignees: []
created_at: 2024-12-24T17:01:26Z
updated_at: 2024-12-30T14:52:37Z
url: https://github.com/astral-sh/ruff/issues/15132
synced_at: 2026-01-10T11:09:56Z
```

# `PIE796` false-positives on enum member cast in pyi files

---

_Issue opened by @Avasam on 2024-12-24 17:01_

Found this whilst applying various Ruff groups on typeshed out of curiosity.

According to the updated typing spec for typing enums (see [Mypy: Change to Enum Membership Semantics](https://mypy-lang.blogspot.com/2024/12/mypy-114-released.html) and https://typing.readthedocs.io/en/latest/spec/enums.html#defining-members, this is the recommended way to type an enum in stubs where the type is known but the value isn't:
```py
class Key(enum.Enum):
    alt = cast(KeyCode, ...)
    alt_l = cast(KeyCode, ...)
    alt_r = cast(KeyCode, ...)
    alt_gr = cast(KeyCode, ...)
    # ...
```
This will trigger `PIE796`

However I think that a fix could be simpler than that: This rule probably shouldn't apply to stubs as it's out of the control of a stub's author (ref https://github.com/astral-sh/ruff/issues/14535 )

ruff 0.8.4
command: ruff check --select=PIE --isolated

---

_Label `bug` added by @dylwil3 on 2024-12-24 21:17_

---

_Label `rule` added by @dylwil3 on 2024-12-24 21:17_

---

_Comment by @InSyncWithFoo on 2024-12-25 10:32_

Rather than disabling the entire rule, I think the pattern should simply be ignored for stub files when checking for duplicates. It is possible that a stub author might add two members with the same value by mistake (say, via a copy and paste error).

---

_Closed by @MichaReiser on 2024-12-30 14:52_

---
