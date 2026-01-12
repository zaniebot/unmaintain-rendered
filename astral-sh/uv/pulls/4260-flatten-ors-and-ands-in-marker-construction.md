```yaml
number: 4260
title: Flatten ORs and ANDs in marker construction
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/norm
created_at: 2024-06-12T01:26:10Z
updated_at: 2024-06-12T01:44:50Z
url: https://github.com/astral-sh/uv/pull/4260
synced_at: 2026-01-12T16:06:07Z
```

# Flatten ORs and ANDs in marker construction

---

_@charliermarsh_

## Summary

If we're ORing an OR, we should just append rather than nesting in another OR.

In my branch, this let us simplify:

```
python_version < '3.10' or python_version > '3.12' or (python_version < '3.8' or python_version > '3.12')
```

To:

```
python_version < '3.10' or python_version > '3.12
```


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-12 01:26_

---

_Label `enhancement` added by @charliermarsh on 2024-06-12 01:26_

---

_@ibraheemdev approved on 2024-06-12 01:36_

Nice! Catching this at construction time is perfect.

---

_Merged by @charliermarsh on 2024-06-12 01:44_

---

_Closed by @charliermarsh on 2024-06-12 01:44_

---

_Branch deleted on 2024-06-12 01:44_

---
