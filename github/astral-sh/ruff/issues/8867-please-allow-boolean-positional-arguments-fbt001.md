---
number: 8867
title: "please allow boolean positional arguments (`FBT001`, `FBT002`) in case of `@override`"
type: issue
state: closed
author: ZeeD
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-11-28T13:53:50Z
updated_at: 2023-11-29T10:38:22Z
url: https://github.com/astral-sh/ruff/issues/8867
synced_at: 2026-01-07T13:12:15-06:00
---

# please allow boolean positional arguments (`FBT001`, `FBT002`) in case of `@override`

---

_Issue opened by @ZeeD on 2023-11-28 13:53_

While generally useful, the rules `FBT001` and `FBT002` may trigger errors if applied to an ovverriden method.

In my case I'm extending a class from a 3rd part library, and I need to override some methods with "naked booleans".

I got the FBT00? errors by ruff in my code, but if I fix them, the original class doesn't work anymore because there are calls to the methods passing the booleans positionally, and they are out of my control.

---

_Comment by @Avasam on 2023-11-28 19:19_

Same reasoning as #3910 and #6958

---

_Label `bug` added by @zanieb on 2023-11-28 19:30_

---

_Label `good first issue` added by @zanieb on 2023-11-28 19:30_

---

_Comment by @charliermarsh on 2023-11-28 20:16_

Makes sense.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-28 20:18_

---

_Referenced in [astral-sh/ruff#8882](../../astral-sh/ruff/pulls/8882.md) on 2023-11-28 21:22_

---

_Closed by @charliermarsh on 2023-11-28 21:42_

---

_Comment by @ZeeD on 2023-11-29 10:38_

Thanks!

---

_Referenced in [astral-sh/ruff#8945](../../astral-sh/ruff/issues/8945.md) on 2023-12-01 10:09_

---
