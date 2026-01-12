```yaml
number: 11229
title: "Disable wheel testing for `powerpc64le-unknown-linux-gnu`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/pcc
created_at: 2025-02-05T03:30:15Z
updated_at: 2025-02-05T04:21:59Z
url: https://github.com/astral-sh/uv/pull/11229
synced_at: 2026-01-12T16:09:44Z
```

# Disable wheel testing for `powerpc64le-unknown-linux-gnu`

---

_@charliermarsh_

## Summary

I need to look into this later, but the test step is failing to install Python: https://github.com/astral-sh/uv/actions/runs/13148286589/job/36694160839. We already disable this for the non-`le` variant, so this seems ok to revisit.


---

_Label `testing` added by @charliermarsh on 2025-02-05 03:30_

---

_Merged by @charliermarsh on 2025-02-05 03:46_

---

_Closed by @charliermarsh on 2025-02-05 03:46_

---

_Branch deleted on 2025-02-05 03:46_

---

_Comment by @zanieb on 2025-02-05 04:21_

Tracking a fix in https://github.com/astral-sh/uv/issues/11231

---
