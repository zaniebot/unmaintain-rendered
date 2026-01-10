```yaml
number: 5980
title: Use consistent canonicalization for URLs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/new-relative-wheel
created_at: 2024-08-09T22:06:52Z
updated_at: 2024-08-10T01:43:38Z
url: https://github.com/astral-sh/uv/pull/5980
synced_at: 2026-01-10T13:31:54Z
```

# Use consistent canonicalization for URLs

---

_Pull request opened by @charliermarsh on 2024-08-09 22:06_

Right now, the URL gets out-of-sync with the install path, since the install path is canonicalized. This leads to a subtle error on Windows (in CI) in which we don't preserve caching across resolution and installation.

---

_Label `testing` added by @charliermarsh on 2024-08-09 22:07_

---

_Comment by @charliermarsh on 2024-08-10 00:07_

Ok the problem here is that the project directory gets canonicalized during workspace discovery...

---

_Marked ready for review by @charliermarsh on 2024-08-10 01:42_

---

_Label `testing` removed by @charliermarsh on 2024-08-10 01:42_

---

_Label `bug` added by @charliermarsh on 2024-08-10 01:42_

---

_Renamed from "Re-enable relative path test on Windows" to "Use consistent canonicalization for URLs" by @charliermarsh on 2024-08-10 01:43_

---

_Merged by @charliermarsh on 2024-08-10 01:43_

---

_Closed by @charliermarsh on 2024-08-10 01:43_

---

_Branch deleted on 2024-08-10 01:43_

---
