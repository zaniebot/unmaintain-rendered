```yaml
number: 9136
title: Show derivation markers for resolutions with project name
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/root
created_at: 2024-11-15T00:06:24Z
updated_at: 2024-11-15T00:25:27Z
url: https://github.com/astral-sh/uv/pull/9136
synced_at: 2026-01-12T16:08:40Z
```

# Show derivation markers for resolutions with project name

---

_@charliermarsh_

## Summary

I was wrongly using `.name()` to detect if a package was "not root", but in `pip compile`, the root can have a name -- so we were failing to find the derivation chain.


---

_Label `bug` added by @charliermarsh on 2024-11-15 00:06_

---

_Marked ready for review by @charliermarsh on 2024-11-15 00:06_

---

_Merged by @charliermarsh on 2024-11-15 00:25_

---

_Closed by @charliermarsh on 2024-11-15 00:25_

---

_Branch deleted on 2024-11-15 00:25_

---
