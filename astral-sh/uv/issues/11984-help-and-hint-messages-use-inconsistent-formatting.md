---
number: 11984
title: Help and hint messages use inconsistent formatting
type: issue
state: open
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2025-03-05T17:12:11Z
updated_at: 2025-03-05T17:22:37Z
url: https://github.com/astral-sh/uv/issues/11984
synced_at: 2026-01-10T01:25:13Z
---

# Help and hint messages use inconsistent formatting

---

_Issue opened by @charliermarsh on 2025-03-05 17:12_

I think `help:` comes from Miette and `hint:` we do ourselves.

![Image](https://github.com/user-attachments/assets/1f049c70-f7b0-41ad-b40a-89a28d4f6651)

---

_Label `cli` added by @charliermarsh on 2025-03-05 17:12_

---

_Comment by @zanieb on 2025-03-05 17:16_

I wonder if we should ever be using `help`?

---

_Comment by @charliermarsh on 2025-03-05 17:16_

Probably not, I think it was a little easier though (in the implementation).

---

_Comment by @zanieb on 2025-03-05 17:22_

We might want to abstract the hint logic ourselves. It's annoying to repeat everywhere.

cc @Gankra

---
