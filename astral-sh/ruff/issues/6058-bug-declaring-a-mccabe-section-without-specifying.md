```yaml
number: 6058
title: "[Bug] Declaring a `[mccabe]` section without specifying `max-complexity` defaults it to `0`"
type: issue
state: closed
author: tylerlaprade
labels:
  - bug
  - configuration
assignees: []
created_at: 2023-07-25T08:57:39Z
updated_at: 2023-07-25T18:10:11Z
url: https://github.com/astral-sh/ruff/issues/6058
synced_at: 2026-01-10T11:09:48Z
```

# [Bug] Declaring a `[mccabe]` section without specifying `max-complexity` defaults it to `0`

---

_Issue opened by @tylerlaprade on 2023-07-25 08:57_

#### Repro:
In my config, I had an empty `[mccabe]` section, i.e. I did not set `max-complexity`.

#### Expected:
Default value should be 10

#### Actual
Value was 0, humorously flagging every single function in my codebase

#### Screenshots
<img width="360" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/da89d628-4aa9-491a-b7b3-43b6bc91c0b2">

<img width="374" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/9038aeb1-530f-489c-a8ae-e08d035a29dc">



---

_Label `bug` added by @charliermarsh on 2023-07-25 14:10_

---

_Label `configuration` added by @charliermarsh on 2023-07-25 14:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-25 15:27_

---

_Closed by @charliermarsh on 2023-07-25 15:38_

---

_Comment by @tylerlaprade on 2023-07-25 18:10_

Thanks for the quick fix, @charliermarsh! Is it possible that other settings are affected in a similar way?

---
