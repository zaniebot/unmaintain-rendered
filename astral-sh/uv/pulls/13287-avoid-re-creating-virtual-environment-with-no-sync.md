```yaml
number: 13287
title: "Avoid re-creating virtual environment with `--no-sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/sync-venv
created_at: 2025-05-04T22:30:19Z
updated_at: 2025-05-05T14:57:48Z
url: https://github.com/astral-sh/uv/pull/13287
synced_at: 2026-01-10T11:10:41Z
```

# Avoid re-creating virtual environment with `--no-sync`

---

_Pull request opened by @charliermarsh on 2025-05-04 22:30_

## Summary

We now show a user-visible warning if we're using a "stale" virtual environment due to `--no-sync`. I'd also be fine erroring here.

Closes https://github.com/astral-sh/uv/issues/13235.


---

_Label `error messages` added by @charliermarsh on 2025-05-04 22:30_

---

_Review requested from @Gankra by @charliermarsh on 2025-05-04 22:30_

---

_Review requested from @zanieb by @charliermarsh on 2025-05-04 22:30_

---

_Marked ready for review by @charliermarsh on 2025-05-04 22:30_

---

_@zanieb approved on 2025-05-05 13:33_

Makes sense to me.

I might say "Using" instead of "Retaining"?

---

_Merged by @charliermarsh on 2025-05-05 14:57_

---

_Closed by @charliermarsh on 2025-05-05 14:57_

---

_Branch deleted on 2025-05-05 14:57_

---
