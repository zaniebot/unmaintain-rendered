```yaml
number: 15759
title: "Use token store credentials for `uv publish`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pub
created_at: 2025-09-09T16:01:03Z
updated_at: 2025-09-09T16:13:33Z
url: https://github.com/astral-sh/uv/pull/15759
synced_at: 2026-01-12T16:11:55Z
```

# Use token store credentials for `uv publish`

---

_@charliermarsh_

## Summary

Running `uv publish` to pyx should re-use the already-stored token rather than prompting for credentials.

Closes https://github.com/astral-sh/uv/issues/15758.

---

_Label `bug` added by @charliermarsh on 2025-09-09 16:01_

---

_Review requested from @zsol by @charliermarsh on 2025-09-09 16:01_

---

_Marked ready for review by @charliermarsh on 2025-09-09 16:01_

---

_@zsol approved on 2025-09-09 16:04_

is `publish` the only place we should do this?

---

_Comment by @charliermarsh on 2025-09-09 16:12_

I believe so, it's already wired up correctly in the middleware.

---

_Merged by @charliermarsh on 2025-09-09 16:13_

---

_Closed by @charliermarsh on 2025-09-09 16:13_

---

_Branch deleted on 2025-09-09 16:13_

---
