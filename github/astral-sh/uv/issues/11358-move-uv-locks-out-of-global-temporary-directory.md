---
number: 11358
title: Move uv locks out of global temporary directory
type: issue
state: open
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2025-02-09T18:13:28Z
updated_at: 2025-02-09T18:13:31Z
url: https://github.com/astral-sh/uv/issues/11358
synced_at: 2026-01-07T13:12:18-06:00
---

# Move uv locks out of global temporary directory

---

_Issue opened by @charliermarsh on 2025-02-09 18:13_

I think we should just store these in the cache or some other persistent data directory as a form of scratch space.

---

_Label `enhancement` added by @charliermarsh on 2025-02-09 18:13_

---
