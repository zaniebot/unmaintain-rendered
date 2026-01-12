```yaml
number: 5868
title: "Assume `git+` prefix when URLs end in `.git`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/g
created_at: 2024-08-07T15:20:48Z
updated_at: 2024-08-07T15:50:37Z
url: https://github.com/astral-sh/uv/pull/5868
synced_at: 2026-01-12T16:07:04Z
```

# Assume `git+` prefix when URLs end in `.git`

---

_@charliermarsh_

## Summary

Right now, this applies _everywhere_, so the following also works:

```
pip install "elmer-circuitbuilder @ https://github.com/ElmerCSC/elmer_circuitbuilder.git"
```

I actually think that's ok?

Closes https://github.com/astral-sh/uv/issues/5866.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-07 15:20_

---

_Review requested from @konstin by @charliermarsh on 2024-08-07 15:20_

---

_Label `enhancement` added by @charliermarsh on 2024-08-07 15:20_

---

_@zanieb approved on 2024-08-07 15:31_

---

_Merged by @charliermarsh on 2024-08-07 15:50_

---

_Closed by @charliermarsh on 2024-08-07 15:50_

---

_Branch deleted on 2024-08-07 15:50_

---
