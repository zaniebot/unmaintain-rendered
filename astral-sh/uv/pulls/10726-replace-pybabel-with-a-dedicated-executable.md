```yaml
number: 10726
title: "Replace `pybabel` with a dedicated executable package"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/tool
created_at: 2025-01-17T23:28:54Z
updated_at: 2025-01-21T17:26:29Z
url: https://github.com/astral-sh/uv/pull/10726
synced_at: 2026-01-12T16:09:27Z
```

# Replace `pybabel` with a dedicated executable package

---

_@charliermarsh_

## Summary

Replacing the large `pybabel` in tests with [`executable-application`](https://pypi.org/project/executable-application/) (1.7 KB).

We may want a separate test package with an executable that _does_ match the name? This one intentionally does _not_. It would make it much easier for us to rewrite the other tests in bulk, since we can do a find-and-replace on `black`, etc.

Closes https://github.com/astral-sh/uv/issues/10646.

---

_Review requested from @zanieb by @charliermarsh on 2025-01-17 23:28_

---

_Label `testing` added by @charliermarsh on 2025-01-17 23:29_

---

_@zanieb approved on 2025-01-21 15:00_

---

_Merged by @charliermarsh on 2025-01-21 17:26_

---

_Closed by @charliermarsh on 2025-01-21 17:26_

---

_Branch deleted on 2025-01-21 17:26_

---
