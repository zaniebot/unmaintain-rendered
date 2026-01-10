```yaml
number: 1476
title: "Issue while resolving versions of editable packages.  `uv pip install  -e {package}`"
type: issue
state: closed
author: moaddib666
labels:
  - bug
assignees: []
created_at: 2024-02-16T10:29:57Z
updated_at: 2024-04-01T21:31:40Z
url: https://github.com/astral-sh/uv/issues/1476
synced_at: 2026-01-10T05:40:31Z
```

# Issue while resolving versions of editable packages.  `uv pip install  -e {package}`

---

_Issue opened by @moaddib666 on 2024-02-16 10:29_

Steps to reproduce 
1. Install package A in editable mode
2. Install package B that relay on package A in editable mode

Expected result:
Packages A and B has been installed.

Actual result:
```
Audited 1 package in 3ms
uv pip install --prerelease=allow -e ecosystem/utils/ecosystem-utils-logging[test]
Audited 1 package in 4ms
uv pip install --prerelease=allow -e ecosystem/utils/ecosystem-utils-configuration[test]
   Built file:///Users/moaddib/PycharmProjects/spaceship/ecosystem.python/ecosystem/utils/ecosystem-utils-configuration                                                                                Built 1 editable in 829ms
  × No solution found when resolving dependencies:
  ╰─▶ Because ecosystem-utils-logging==0.0.1a0 was not found in the package registry and ecosystem-utils-configuration==0.0.1a0 depends on ecosystem-utils-logging==0.0.1a0, we can conclude that
      ecosystem-utils-configuration==0.0.1a0 cannot be used.
      And because you require ecosystem-utils-configuration==0.0.1a0, we can conclude that the requirements are unsatisfiable.
make: *** [install] Error 1
```

---

_Label `bug` added by @charliermarsh on 2024-02-17 01:32_

---

_Comment by @charliermarsh on 2024-02-22 03:14_

This is the same as https://github.com/astral-sh/uv/issues/1661. (Gonna merge into that issue. This one was first, but I wrote up some notes on the other issue by chance.)

---

_Closed by @charliermarsh on 2024-02-22 03:14_

---

_Comment by @zanieb on 2024-04-01 21:31_

Hi! This should be addressed in the latest release ([0.1.27](https://github.com/astral-sh/uv/releases/tag/0.1.27)) via #2596.

Let us know if you have any problems.

---
