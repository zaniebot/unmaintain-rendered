```yaml
number: 3821
title: Always include package names for Git and HTTPS dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unnamed
created_at: 2024-05-24T13:47:57Z
updated_at: 2024-05-24T14:01:39Z
url: https://github.com/astral-sh/uv/pull/3821
synced_at: 2026-01-10T14:32:20Z
```

# Always include package names for Git and HTTPS dependencies

---

_Pull request opened by @charliermarsh on 2024-05-24 13:47_

## Summary

Related to https://github.com/astral-sh/uv/issues/3818. We should _always_ include the package name if we know it's not a file path, even if it starts with an environment variable.

Closes https://github.com/astral-sh/uv/pull/3821, although there's another bug to fix there for _local_ dependencies that I need to PR separately.


---

_Label `bug` added by @charliermarsh on 2024-05-24 13:49_

---

_Merged by @charliermarsh on 2024-05-24 14:01_

---

_Closed by @charliermarsh on 2024-05-24 14:01_

---

_Branch deleted on 2024-05-24 14:01_

---
