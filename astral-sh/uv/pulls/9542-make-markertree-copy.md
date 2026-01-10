```yaml
number: 9542
title: "Make `MarkerTree` `Copy`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/copy
created_at: 2024-11-30T15:36:59Z
updated_at: 2024-11-30T19:07:09Z
url: https://github.com/astral-sh/uv/pull/9542
synced_at: 2026-01-10T12:00:00Z
```

# Make `MarkerTree` `Copy`

---

_Pull request opened by @charliermarsh on 2024-11-30 15:36_

## Summary

It's just a `usize`. It seems simpler and perhaps even more performant (?) to make it `Copy`.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-30 15:37_

---

_Review requested from @konstin by @charliermarsh on 2024-11-30 15:37_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-11-30 15:37_

---

_Label `internal` added by @charliermarsh on 2024-11-30 15:37_

---

_@ibraheemdev approved on 2024-11-30 17:59_

I left this off before to avoid churn while the implementation was still in flux, but this makes sense now.

---

_Merged by @charliermarsh on 2024-11-30 19:07_

---

_Closed by @charliermarsh on 2024-11-30 19:07_

---

_Branch deleted on 2024-11-30 19:07_

---
