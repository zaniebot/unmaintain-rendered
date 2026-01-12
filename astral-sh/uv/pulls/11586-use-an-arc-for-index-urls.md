```yaml
number: 11586
title: "Use an `Arc` for Index URLs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/ar
created_at: 2025-02-18T02:13:51Z
updated_at: 2025-02-18T02:22:37Z
url: https://github.com/astral-sh/uv/pull/11586
synced_at: 2026-01-12T16:09:54Z
```

# Use an `Arc` for Index URLs

---

_@charliermarsh_

## Summary

I realized that we clone this for every distribution. It looks like it's consistently 1-2% faster to use an `Arc` here.


---

_Label `performance` added by @charliermarsh on 2025-02-18 02:13_

---

_Merged by @charliermarsh on 2025-02-18 02:22_

---

_Closed by @charliermarsh on 2025-02-18 02:22_

---

_Branch deleted on 2025-02-18 02:22_

---
