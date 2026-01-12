```yaml
number: 11144
title: "Migrate from `urlencoding` to `percent-encoding`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/percent-encoding
created_at: 2025-01-31T20:28:29Z
updated_at: 2025-01-31T21:29:48Z
url: https://github.com/astral-sh/uv/pull/11144
synced_at: 2026-01-12T16:09:42Z
```

# Migrate from `urlencoding` to `percent-encoding`

---

_@charliermarsh_

## Summary

This lets us drop a dependency entirely. `percent-encoding` is used by `url` and so is already in the graph, whereas `urlencoding` isn't used by anything else.


---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-31 20:28_

---

_Review requested from @Gankra by @charliermarsh on 2025-01-31 20:28_

---

_Label `internal` added by @charliermarsh on 2025-01-31 20:28_

---

_@BurntSushi approved on 2025-01-31 21:15_

---

_Merged by @charliermarsh on 2025-01-31 21:29_

---

_Closed by @charliermarsh on 2025-01-31 21:29_

---

_Branch deleted on 2025-01-31 21:29_

---
