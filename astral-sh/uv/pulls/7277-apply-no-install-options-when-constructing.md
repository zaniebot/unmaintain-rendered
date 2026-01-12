```yaml
number: 7277
title: "Apply `--no-install` options when constructing resolution"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/only-dev
created_at: 2024-09-10T21:54:45Z
updated_at: 2024-09-11T21:20:46Z
url: https://github.com/astral-sh/uv/pull/7277
synced_at: 2026-01-12T16:07:46Z
```

# Apply `--no-install` options when constructing resolution

---

_@charliermarsh_

## Summary

We need to apply the `--no-install` filters earlier, such that we don't error if we only have a source distribution for a given package when `--no-build` is provided but that package is _omitted_.

Closes #7247.


---

_Label `bug` added by @charliermarsh on 2024-09-10 21:54_

---

_Marked ready for review by @charliermarsh on 2024-09-10 21:54_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-10 22:10_

---

_@zanieb approved on 2024-09-11 18:31_

---

_Merged by @charliermarsh on 2024-09-11 18:31_

---

_Closed by @charliermarsh on 2024-09-11 18:31_

---

_Branch deleted on 2024-09-11 18:31_

---

_Comment by @neutrinoceros on 2024-09-11 21:20_

Thanks a lot for fixing this so quickly !

---
