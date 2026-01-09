---
number: 12119
title: "`[index]` fields are missing from the settings reference"
type: issue
state: open
author: zanieb
labels:
  - bug
  - documentation
assignees: []
created_at: 2025-03-11T19:59:47Z
updated_at: 2025-03-17T20:49:25Z
url: https://github.com/astral-sh/uv/issues/12119
synced_at: 2026-01-07T13:12:18-06:00
---

# `[index]` fields are missing from the settings reference

---

_Issue opened by @zanieb on 2025-03-11 19:59_

### Summary

As mentioned in https://github.com/astral-sh/uv/pull/12102 — these fields are missing from the generated settings reference.

### Platform

n/a

### Version

n/a

### Python version

_No response_

---

_Label `bug` added by @zanieb on 2025-03-11 19:59_

---

_Label `documentation` added by @zanieb on 2025-03-11 19:59_

---

_Assigned to @charliermarsh by @zanieb on 2025-03-11 20:00_

---

_Comment by @charliermarsh on 2025-03-13 22:03_

Will take a look tonight.

---

_Comment by @charliermarsh on 2025-03-13 23:44_

@zanieb -- I took a look -- I don't think we support expanding these though? At least, we would have to get rid of the entry for `index` itself and only show the fields within it, which seems like a loss. (Look at `workspace` as an example of that.)


---

_Comment by @charliermarsh on 2025-03-13 23:45_

Should we just add the auth policy docs on there directly?

---

_Comment by @zanieb on 2025-03-17 20:49_

Can we not add support for both showing `index` and expanding it? It seems like a good investment.

---
