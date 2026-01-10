```yaml
number: 6395
title: "Treat invalid extras as `false` in marker evaluation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/underscore
created_at: 2024-08-22T00:34:44Z
updated_at: 2024-08-23T10:30:43Z
url: https://github.com/astral-sh/uv/pull/6395
synced_at: 2026-01-10T13:09:51Z
```

# Treat invalid extras as `false` in marker evaluation

---

_Pull request opened by @charliermarsh on 2024-08-22 00:34_

## Summary

Closes https://github.com/astral-sh/uv/issues/6279.

Closes https://github.com/astral-sh/uv/pull/6395.


---

_Label `bug` added by @charliermarsh on 2024-08-22 00:34_

---

_@zanieb reviewed on 2024-08-22 00:37_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:11857 on 2024-08-22 00:37_

Can you add test coverage for requesting the `anyio` extra explicitly?

---

_Merged by @charliermarsh on 2024-08-22 01:03_

---

_Closed by @charliermarsh on 2024-08-22 01:03_

---

_Branch deleted on 2024-08-22 01:03_

---

_Comment by @AKuederle on 2024-08-23 10:30_

I just retested #6324 with uv 0.3.2 and it seems like it is still behaving as before. So the additional packages are still installed. The only difference that I see between #6324 and #6279 is that in my case I had a leading "-" and in the other case there was a leading "_"

---
