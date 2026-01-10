```yaml
number: 15292
title: "Reject `match-runtime = true` for dynamic packages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/match
created_at: 2025-08-14T23:08:58Z
updated_at: 2025-08-15T09:18:12Z
url: https://github.com/astral-sh/uv/pull/15292
synced_at: 2026-01-10T06:44:33Z
```

# Reject `match-runtime = true` for dynamic packages

---

_Pull request opened by @charliermarsh on 2025-08-14 23:08_

## Summary

If `match-runtime = true`, but we can't resolve a package's metadata statically, then we can't _know_ what the runtime version of the package will be -- because we can't resolve without building it. This PR makes that footgun clearer by raising an error.

Closes https://github.com/astral-sh/uv/issues/15264.


---

_Review requested from @zanieb by @charliermarsh on 2025-08-14 23:09_

---

_Label `error messages` added by @charliermarsh on 2025-08-14 23:09_

---

_Marked ready for review by @charliermarsh on 2025-08-14 23:09_

---

_@zanieb approved on 2025-08-15 03:53_

---

_Merged by @charliermarsh on 2025-08-15 09:18_

---

_Closed by @charliermarsh on 2025-08-15 09:18_

---

_Branch deleted on 2025-08-15 09:18_

---
