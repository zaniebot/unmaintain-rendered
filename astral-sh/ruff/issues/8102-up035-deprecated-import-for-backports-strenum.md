```yaml
number: 8102
title: UP035 (deprecated-import) for backports.strenum
type: issue
state: closed
author: amartani
labels:
  - wish
assignees: []
created_at: 2023-10-21T03:02:08Z
updated_at: 2023-10-21T23:13:05Z
url: https://github.com/astral-sh/ruff/issues/8102
synced_at: 2026-01-12T15:54:47Z
```

# UP035 (deprecated-import) for backports.strenum

---

_@amartani_

https://pypi.org/project/backports.strenum/ implements a backport of Python 3.11's `enum.StrEnum` as `backports.strenum.StrEnum`. It would be nice if `UP035` (deprecated-import) added support for replacing it with the original version when targeting 3.11+.

Note that the package instructions already suggests using:

```
if sys.version_info >= (3, 11):
    from enum import StrEnum
else:
    from backports.strenum import StrEnum
```

But if someone is not using the conditional import, it would be good for UP035 to clean it up.

Tested on ruff v0.1.1 - https://play.ruff.rs/c973b82b-0107-469b-9b10-9196353b8901 

---

_Label `wish` added by @charliermarsh on 2023-10-21 19:31_

---

_Comment by @charliermarsh on 2023-10-21 19:31_

Seems reasonable!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-21 23:00_

---

_Closed by @charliermarsh on 2023-10-21 23:13_

---
