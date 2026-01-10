---
number: 4470
title: "Consider versioning `tool.uv` schema"
type: issue
state: closed
author: charliermarsh
labels:
  - configuration
assignees: []
created_at: 2024-06-24T13:11:56Z
updated_at: 2024-07-30T17:21:35Z
url: https://github.com/astral-sh/uv/issues/4470
synced_at: 2026-01-10T01:23:38Z
---

# Consider versioning `tool.uv` schema

---

_Issue opened by @charliermarsh on 2024-06-24 13:11_

Do we want to version the schema separately from `uv` itself, so that we can support different formats over time?

---

_Label `configuration` added by @charliermarsh on 2024-06-24 13:12_

---

_Comment by @zanieb on 2024-06-24 13:23_

It'd be nice, but I'm not sure it's a high priority in the short term. It seems non-trivial to implement right?

---

_Comment by @charliermarsh on 2024-06-24 13:28_

Adding the version is trivial. The hard work comes when we make a breaking change and bump it. The reason to consider it now is that it may be hard to add retroactively (or not, but we should just think that through while we can).

---

_Comment by @charliermarsh on 2024-07-29 01:12_

We don't need to do this now.

---

_Closed by @charliermarsh on 2024-07-29 01:12_

---

_Comment by @zanieb on 2024-07-30 14:30_

Can you note why?

---

_Comment by @charliermarsh on 2024-07-30 17:21_

We may want to introduce this later, but I don't know that it buys us much to introduce it to start, especially since we can't _require_ it.

---
