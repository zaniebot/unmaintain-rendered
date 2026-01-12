```yaml
number: 9966
title: How to use uv with alembic?
type: issue
state: closed
author: raibann
labels:
  - question
assignees: []
created_at: 2024-12-17T11:14:03Z
updated_at: 2024-12-27T14:36:27Z
url: https://github.com/astral-sh/uv/issues/9966
synced_at: 2026-01-12T16:00:03Z
```

# How to use uv with alembic?

---

_@raibann_

<img width="407" alt="Image" src="https://github.com/user-attachments/assets/0a9abe3f-c6fe-4e65-890e-d043fdcb71d1" />
it not work how ever put it in package or project it still not work
- dep:
 alembic
sqlachemy
- project type:( monorepo )

---

_Comment by @flyaroundme on 2024-12-17 21:36_

How do you launch it?

---

_Comment by @samypr100 on 2024-12-19 01:01_

In a project setup like this, you can use `uv run alembic <args>` assuming it's part of your lock dependencies.

---

_Label `question` added by @samypr100 on 2024-12-19 01:01_

---

_Closed by @charliermarsh on 2024-12-27 14:36_

---
