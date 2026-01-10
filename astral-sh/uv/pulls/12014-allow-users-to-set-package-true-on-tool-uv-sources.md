```yaml
number: 12014
title: "Allow users to set `package = true` on `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/p
created_at: 2025-03-06T18:11:16Z
updated_at: 2025-03-06T18:28:10Z
url: https://github.com/astral-sh/uv/pull/12014
synced_at: 2026-01-10T11:10:39Z
```

# Allow users to set `package = true` on `tool.uv.sources`

---

_Pull request opened by @charliermarsh on 2025-03-06 18:11_

## Summary

In https://github.com/astral-sh/uv/issues/11998, a user is attempting to vendor `pydantic-core`. But when they add `pydantic-core = { path = "src/foo/vendor/pydantic-core" } `, we're installing it as a virtual package, since `pydantic-core/pyproject.toml` contains `package = false`.

This PR allows users to mark dependencies as "explicitly a package" or "explicitly not a package" (i.e., virtual), as a workaround.

Closes https://github.com/astral-sh/uv/issues/11998.


---

_Label `enhancement` added by @charliermarsh on 2025-03-06 18:11_

---

_@zanieb approved on 2025-03-06 18:12_

---

_Merged by @charliermarsh on 2025-03-06 18:28_

---

_Closed by @charliermarsh on 2025-03-06 18:28_

---

_Branch deleted on 2025-03-06 18:28_

---
