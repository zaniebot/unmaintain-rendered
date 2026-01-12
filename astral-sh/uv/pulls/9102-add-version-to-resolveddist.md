```yaml
number: 9102
title: "Add `version` to `ResolvedDist`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/version
created_at: 2024-11-13T23:17:45Z
updated_at: 2024-11-14T00:06:18Z
url: https://github.com/astral-sh/uv/pull/9102
synced_at: 2026-01-12T16:08:39Z
```

# Add `version` to `ResolvedDist`

---

_@charliermarsh_

## Summary

I need this for the derivation chain work (https://github.com/astral-sh/uv/issues/8962), but it just seems generally useful. You can't always get a version from a `Dist` (it could be URL-based!), but when we create a `ResolvedDist`, we _do_ know the version (and not just the URL). This PR preserves it.


---

_Label `internal` added by @charliermarsh on 2024-11-13 23:17_

---

_Marked ready for review by @charliermarsh on 2024-11-13 23:26_

---

_Merged by @charliermarsh on 2024-11-14 00:06_

---

_Closed by @charliermarsh on 2024-11-14 00:06_

---

_Branch deleted on 2024-11-14 00:06_

---
