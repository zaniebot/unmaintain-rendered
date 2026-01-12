```yaml
number: 4263
title: Deduplicate markers during normalization
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dedup
created_at: 2024-06-12T02:28:55Z
updated_at: 2024-06-12T02:38:16Z
url: https://github.com/astral-sh/uv/pull/4263
synced_at: 2026-01-12T16:06:07Z
```

# Deduplicate markers during normalization

---

_@charliermarsh_

## Summary

We need to sort _before_ deduplicating; otherwise, we can't detect adjacent elements, so we aren't guaranteed to deduplicate anything.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-12 02:28_

---

_Label `bug` added by @charliermarsh on 2024-06-12 02:28_

---

_Merged by @charliermarsh on 2024-06-12 02:38_

---

_Closed by @charliermarsh on 2024-06-12 02:38_

---

_Branch deleted on 2024-06-12 02:38_

---
