```yaml
number: 8673
title: "Fix flaky failure in `run_from_directory`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/r
created_at: 2024-10-29T17:37:38Z
updated_at: 2024-10-29T17:45:00Z
url: https://github.com/astral-sh/uv/pull/8673
synced_at: 2026-01-12T16:08:26Z
```

# Fix flaky failure in `run_from_directory`

---

_@charliermarsh_

## Summary

See, e.g., https://github.com/astral-sh/uv/actions/runs/11578473249/job/32232394993.

I suspect this is some sort of timing flakiness in the cache, so let's just clear the environment for each run.


---

_Label `testing` added by @charliermarsh on 2024-10-29 17:37_

---

_@zanieb approved on 2024-10-29 17:44_

Thanks!

---

_Merged by @charliermarsh on 2024-10-29 17:44_

---

_Closed by @charliermarsh on 2024-10-29 17:44_

---

_Branch deleted on 2024-10-29 17:45_

---
