---
number: 1605
title: Replace/remove imports as in reorder_python_imports
type: issue
state: closed
author: twoertwein
labels:
  - plugin
assignees: []
created_at: 2023-01-03T17:22:55Z
updated_at: 2023-02-03T18:26:24Z
url: https://github.com/astral-sh/ruff/issues/1605
synced_at: 2026-01-10T01:22:39Z
---

# Replace/remove imports as in reorder_python_imports

---

_Issue opened by @twoertwein on 2023-01-03 17:22_

[reorder_python_imports](https://github.com/asottile/reorder_python_imports/) replaces/removes a few (soon to be) deprecated imports. Might be nice to add those rules to ruff.

---

_Label `plugin` added by @charliermarsh on 2023-01-03 18:38_

---

_Comment by @charliermarsh on 2023-02-03 18:26_

I believe we actually do most of these now via the various `pyupgrade` rules. If something specific is missing, always happy to revisit.

---

_Closed by @charliermarsh on 2023-02-03 18:26_

---
