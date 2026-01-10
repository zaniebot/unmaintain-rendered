```yaml
number: 14195
title: Avoid rendering desugared prefix matches in error messages
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/star
created_at: 2025-06-21T23:04:45Z
updated_at: 2025-06-27T18:06:20Z
url: https://github.com/astral-sh/uv/pull/14195
synced_at: 2026-01-10T06:53:01Z
```

# Avoid rendering desugared prefix matches in error messages

---

_Pull request opened by @charliermarsh on 2025-06-21 23:04_

## Summary

When the user provides a requirement like `==2.4.*`, we desugar that to `>=2.4.dev0,<2.5.dev0`. These bounds then appear in error messages, and worse, they also trick the error message reporter into thinking that the user asked for a pre-release.

This PR adds logic to convert to the more-concise `==2.4.*` representation when possible. We could probably do a similar thing for the compatible release operator (`~=`).

Closes https://github.com/astral-sh/uv/issues/14177.


---

_Label `bug` added by @charliermarsh on 2025-06-21 23:04_

---

_Label `error messages` added by @charliermarsh on 2025-06-21 23:04_

---

_Review requested from @zanieb by @charliermarsh on 2025-06-21 23:43_

---

_Review requested from @Gankra by @charliermarsh on 2025-06-21 23:43_

---

_@zanieb approved on 2025-06-27 17:56_

Sick

---

_Comment by @zanieb on 2025-06-27 17:58_

I rebased a conflict in the test case.

---

_Merged by @zanieb on 2025-06-27 18:06_

---

_Closed by @zanieb on 2025-06-27 18:06_

---

_Branch deleted on 2025-06-27 18:06_

---
