```yaml
number: 1855
title: "Improve spacing preservation for `C405` fixes"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/layout
created_at: 2023-01-13T18:10:41Z
updated_at: 2023-01-13T18:11:10Z
url: https://github.com/astral-sh/ruff/pull/1855
synced_at: 2026-01-12T15:55:07Z
```

# Improve spacing preservation for `C405` fixes

---

_@charliermarsh_

We now preserve the spacing of the more common form:

```py
set((
    1,
))
```

Rather than the less common form:

```py
set(
    (1,)
)
```

---

_Merged by @charliermarsh on 2023-01-13 18:11_

---

_Closed by @charliermarsh on 2023-01-13 18:11_

---

_Branch deleted on 2023-01-13 18:11_

---
