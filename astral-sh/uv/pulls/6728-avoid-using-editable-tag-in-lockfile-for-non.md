```yaml
number: 6728
title: Avoid using editable tag in lockfile for non-package dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2024-08-27T23:08:33Z
updated_at: 2024-08-28T01:19:06Z
url: https://github.com/astral-sh/uv/pull/6728
synced_at: 2026-01-12T16:07:30Z
```

# Avoid using editable tag in lockfile for non-package dependencies

---

_@charliermarsh_

## Summary

Use a dedicated source type for non-package requirements. Also enables us to support non-package `path` dependencies _and_ removes the need to have the member `pyproject.toml` files available when we sync _and_ makes it explicit which dependencies are virtual vs. not (as evidenced by the snapshot changes). All good things!


---

_Review requested from @zanieb by @charliermarsh on 2024-08-27 23:08_

---

_Label `enhancement` added by @charliermarsh on 2024-08-27 23:08_

---

_@zanieb approved on 2024-08-27 23:33_

---

_Merged by @charliermarsh on 2024-08-28 01:19_

---

_Closed by @charliermarsh on 2024-08-28 01:19_

---

_Branch deleted on 2024-08-28 01:19_

---
