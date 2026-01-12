```yaml
number: 4482
title: Skip submodule update for fresh clones
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/sub
created_at: 2024-06-24T19:08:25Z
updated_at: 2024-06-24T21:09:15Z
url: https://github.com/astral-sh/uv/pull/4482
synced_at: 2026-01-12T16:06:15Z
```

# Skip submodule update for fresh clones

---

_@charliermarsh_

## Summary

We unconditionally update the submodules in our Git code, but AFAICT it shouldn't be necessary if we already have a complete, up-to-date fetch available.

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-24 19:08_

---

_Label `performance` added by @charliermarsh on 2024-06-24 19:09_

---

_@ibraheemdev approved on 2024-06-24 21:08_

This seems correct to me, we call `.reset()` for every checkout.

---

_Merged by @charliermarsh on 2024-06-24 21:09_

---

_Closed by @charliermarsh on 2024-06-24 21:09_

---

_Branch deleted on 2024-06-24 21:09_

---
