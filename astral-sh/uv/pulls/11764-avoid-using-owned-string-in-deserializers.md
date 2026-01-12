```yaml
number: 11764
title: "Avoid using owned `String` in deserializers"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/p-1
created_at: 2025-02-25T05:41:42Z
updated_at: 2025-02-25T14:28:18Z
url: https://github.com/astral-sh/uv/pull/11764
synced_at: 2026-01-12T16:09:59Z
```

# Avoid using owned `String` in deserializers

---

_@charliermarsh_

## Summary

This is the pattern I see in a variety of crates, and I believe this is preferred if you don't _need_ an owned `String`, since you can avoid the allocation. This could be pretty impactful for us?


---

_Converted to draft by @charliermarsh on 2025-02-25 05:46_

---

_Marked ready for review by @charliermarsh on 2025-02-25 06:11_

---

_Label `performance` added by @charliermarsh on 2025-02-25 06:17_

---

_Merged by @charliermarsh on 2025-02-25 14:28_

---

_Closed by @charliermarsh on 2025-02-25 14:28_

---

_Branch deleted on 2025-02-25 14:28_

---
