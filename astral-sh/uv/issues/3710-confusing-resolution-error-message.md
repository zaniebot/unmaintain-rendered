---
number: 3710
title: Confusing resolution error message
type: issue
state: closed
author: ibraheemdev
labels:
  - error messages
  - resolver
assignees: []
created_at: 2024-05-21T17:49:43Z
updated_at: 2024-05-21T19:28:24Z
url: https://github.com/astral-sh/uv/issues/3710
synced_at: 2026-01-10T01:23:30Z
---

# Confusing resolution error message

---

_Issue opened by @ibraheemdev on 2024-05-21 17:49_

From [this CI failure](https://github.com/astral-sh/uv/actions/runs/9178311640/job/25237925377?pr=3643#step:6:361):

>   × No solution found when resolving dependencies:
  ╰─▶ Because pandas-stubs==2.0.3.230814 depends on numpy>=1.25.0 and you
      require numpy==1.24.4, we can conclude that you require==0a0.dev0 and
      pandas-stubs==2.0.3.230814 are incompatible.
      And because you require pandas-stubs==2.0.3.230814, we can conclude that
      the requirements are unsatisfiable.
 
 The requirements.txt was generated on a different platform than CI, should be able to reproduce by running `pip install` on [this file](https://github.com/astral-sh/uv/blob/e557cb4cedad811b9dc7e3a33da60154d187cd3c/scripts/requirements/compiled/airflow.txt), on Ubuntu (though there may be some other marker environments involved).

---

_Label `error messages` added by @ibraheemdev on 2024-05-21 17:50_

---

_Label `resolver` added by @ibraheemdev on 2024-05-21 17:50_

---

_Comment by @zanieb on 2024-05-21 17:51_

Fixing the bad error message

---

_Referenced in [astral-sh/uv#3711](../../astral-sh/uv/pulls/3711.md) on 2024-05-21 17:55_

---

_Closed by @zanieb on 2024-05-21 19:28_

---
