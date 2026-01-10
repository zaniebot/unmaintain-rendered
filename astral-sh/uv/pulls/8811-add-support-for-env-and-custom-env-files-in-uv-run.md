```yaml
number: 8811
title: "Add support for `.env` and custom env files in `uv run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/dot
created_at: 2024-11-04T17:38:30Z
updated_at: 2024-11-04T19:26:07Z
url: https://github.com/astral-sh/uv/pull/8811
synced_at: 2026-01-10T12:00:00Z
```

# Add support for `.env` and custom env files in `uv run`

---

_Pull request opened by @charliermarsh on 2024-11-04 17:38_

## Summary

This PR pulls in https://github.com/astral-sh/uv/pull/8263 and https://github.com/astral-sh/uv/pull/8463, which were originally merged into the v0.5 tracking branch but can now be committed separately, as we've made `.env` loading opt-in.

In summary:

- `.env` loading is now opt-in (`--env-file .env`).
- `.env` remains supported on `uv run`, so it's meant for providing environment variables to the run command, rather than to uv itself.


---

_Label `enhancement` added by @charliermarsh on 2024-11-04 17:39_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-04 18:01_

---

_Marked ready for review by @charliermarsh on 2024-11-04 18:01_

---

_@zanieb approved on 2024-11-04 19:20_

---

_Merged by @charliermarsh on 2024-11-04 19:26_

---

_Closed by @charliermarsh on 2024-11-04 19:26_

---

_Branch deleted on 2024-11-04 19:26_

---
