```yaml
number: 17486
title: Normalize test snapshots
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/insta-regenerate
created_at: 2026-01-15T15:33:34Z
updated_at: 2026-01-15T16:50:14Z
url: https://github.com/astral-sh/uv/pull/17486
synced_at: 2026-01-15T17:50:37Z
```

# Normalize test snapshots

---

_@konstin_

This reduces the churn when changing test snapshots, as insta updates the quotes when the contents change. Instead, we decouple it by updating the quotes in bulk here.

Created by:

```
cargo insta test --accept --force-update-snapshots
```


---

_Label `internal` added by @konstin on 2026-01-15 15:33_

---

_@zanieb approved on 2026-01-15 16:25_

---

_Comment by @zanieb on 2026-01-15 16:25_

Some important looking failures

---

_Comment by @konstin on 2026-01-15 16:34_

I was using the latest version of cargo insta, and hit a fixed but unreleased bug: https://github.com/mitsuhiko/insta/pull/858

---

_@gaborbernat approved on 2026-01-15 16:48_

---

_Merged by @konstin on 2026-01-15 16:50_

---

_Closed by @konstin on 2026-01-15 16:50_

---

_Branch deleted on 2026-01-15 16:50_

---
