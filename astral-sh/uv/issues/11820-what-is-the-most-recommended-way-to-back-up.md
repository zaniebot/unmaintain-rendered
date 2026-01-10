---
number: 11820
title: what is the most recommended way to back up / version control venv created by uv
type: issue
state: closed
author: binary-husky
labels:
  - question
assignees: []
created_at: 2025-02-27T04:12:27Z
updated_at: 2025-03-02T10:50:02Z
url: https://github.com/astral-sh/uv/issues/11820
synced_at: 2026-01-10T01:25:11Z
---

# what is the most recommended way to back up / version control venv created by uv

---

_Issue opened by @binary-husky on 2025-02-27 04:12_

### Question

what is the most recommended way to back up / version control venv created by uv?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @binary-husky on 2025-02-27 04:12_

---

_Comment by @konstin on 2025-02-27 09:03_

We recommend not backing up venvs and not adding them to version control. Instead, the venv should be treated as ephemeral, something you can always recreate with `uv sync`. The "source of truth" would be `uv.lock` which contains all the information about how to create the venv and should be checked into the git repository.

---

_Comment by @binary-husky on 2025-03-02 10:50_

> We recommend not backing up venvs and not adding them to version control. Instead, the venv should be treated as ephemeral, something you can always recreate with `uv sync`. The "source of truth" would be `uv.lock` which contains all the information about how to create the venv and should be checked into the git repository.

thank you

---

_Closed by @binary-husky on 2025-03-02 10:50_

---
