```yaml
number: 3508
title: Explore using a dev drive for windows CI
type: issue
state: closed
author: konstin
labels:
  - internal
  - windows
assignees: []
created_at: 2024-05-10T14:40:46Z
updated_at: 2024-05-13T16:15:55Z
url: https://github.com/astral-sh/uv/issues/3508
synced_at: 2026-01-12T15:58:44Z
```

# Explore using a dev drive for windows CI

---

_@konstin_

We observe that windows is much slower on any io operations, e.g. just downloading and unpacking the python versions takes 1.5min. We should explore using using a VHDX virtual harddisk as described in https://github.com/actions/cache/issues/752#issuecomment-1847036770 to see if this solves the performance bottleneck in github actions.

---

_Comment by @notatallshaw on 2024-05-10 16:55_

FYI there are some details here where this was reccomended for pip to do: https://github.com/pypa/pip/issues/12055

Ultimately pip maintainers are very conservative about adding new features, but you might find the post informative.

---

_Label `internal` added by @charliermarsh on 2024-05-10 18:40_

---

_Label `windows` added by @charliermarsh on 2024-05-10 18:40_

---

_Comment by @zanieb on 2024-05-11 02:18_

See also, this brief GitHub Actions thread requesting this feature: https://github.com/actions/runner-images/issues/8698

---

_Closed by @zanieb on 2024-05-13 16:15_

---
