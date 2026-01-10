```yaml
number: 4480
title: Ruff --fix on COM812 introducing TypeError
type: issue
state: closed
author: fennerm
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-05-17T21:14:42Z
updated_at: 2023-05-18T14:35:43Z
url: https://github.com/astral-sh/ruff/issues/4480
synced_at: 2026-01-10T11:09:47Z
```

# Ruff --fix on COM812 introducing TypeError

---

_Issue opened by @fennerm on 2023-05-17 21:14_

I think I'm seeing some buggy behavior with the COM812 rule (running ruff v0.0.267).

Minimal example:
```
from collections import namedtuple

foo = namedtuple(
    name="foo",
    status="bar",
    message="sfdsdfsdgs fsdfsdf output!dsfdfsdjkg  ghfskdjghkdssd sd fsdf  s\n"[
        :20
    ],
)
```
Ruff returns error: `7:13: COM812 [*] Trailing comma missing`. `ruff --fix` inserts a comma after `:20` which is a TypeError.


---

_Label `bug` added by @charliermarsh on 2023-05-17 21:15_

---

_Label `autofix` added by @charliermarsh on 2023-05-17 21:15_

---

_Label `autofix-error` added by @charliermarsh on 2023-05-17 21:15_

---

_Comment by @charliermarsh on 2023-05-17 21:15_

Thanks for reporting!

---

_Comment by @JonathanPlasse on 2023-05-17 21:16_

I can work on it.

---

_Assigned to @JonathanPlasse by @charliermarsh on 2023-05-17 21:16_

---

_Closed by @charliermarsh on 2023-05-18 14:35_

---
