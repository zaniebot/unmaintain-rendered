```yaml
number: 11158
title: Optimize exclusion computation for markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2025-02-02T00:20:21Z
updated_at: 2025-02-02T13:21:33Z
url: https://github.com/astral-sh/uv/pull/11158
synced_at: 2026-01-12T16:09:42Z
```

# Optimize exclusion computation for markers

---

_@charliermarsh_

## Summary

Oddly this showed up in a trace. I think the lack of memoization was making it fairly expensive.


---

_Label `performance` added by @charliermarsh on 2025-02-02 00:20_

---

_@konstin approved on 2025-02-02 11:01_

---

_Merged by @charliermarsh on 2025-02-02 13:21_

---

_Closed by @charliermarsh on 2025-02-02 13:21_

---

_Branch deleted on 2025-02-02 13:21_

---
