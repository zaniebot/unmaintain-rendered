```yaml
number: 2203
title: configurable list for A001 exceptions
type: issue
state: closed
author: spaceone
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2023-01-26T17:16:28Z
updated_at: 2023-01-27T04:17:37Z
url: https://github.com/astral-sh/ruff/issues/2203
synced_at: 2026-01-10T11:09:45Z
```

# configurable list for A001 exceptions

---

_Issue opened by @spaceone on 2023-01-26 17:16_

the config could have a list of exceptions for A001, A002, A003 builtin-variable-shadowing.
For example `license` and `copyright` are not very useful and can imho be overwritten.

---

_Comment by @spaceone on 2023-01-26 17:18_

See also https://docs.python.org/3/library/constants.html#constants-added-by-the-site-module

These constants aren't even available always.

---

_Label `configuration` added by @charliermarsh on 2023-01-26 17:18_

---

_Comment by @charliermarsh on 2023-01-26 17:18_

Makes sense. PR welcome :) I likely won't get to it today.

---

_Comment by @spaceone on 2023-01-26 17:21_

for me this is not urgent, can wait for a couple of month. maybe a good first-issue for someone.

---

_Label `good first issue` added by @charliermarsh on 2023-01-26 17:22_

---

_Comment by @charliermarsh on 2023-01-26 17:22_

Good call :)

---

_Comment by @axitkhurana on 2023-01-27 03:56_

Is that what https://github.com/charliermarsh/ruff#builtins-ignorelist is for or did I misunderstand that config?

---

_Comment by @charliermarsh on 2023-01-27 04:17_

Oh, you're right, that's exactly what it's for. I just totally forgot we supported it >.<

---

_Closed by @charliermarsh on 2023-01-27 04:17_

---
