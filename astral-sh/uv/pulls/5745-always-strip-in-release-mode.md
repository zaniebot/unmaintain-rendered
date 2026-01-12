```yaml
number: 5745
title: Always strip in release mode
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/strip-properly
created_at: 2024-08-03T10:39:10Z
updated_at: 2024-08-03T13:42:47Z
url: https://github.com/astral-sh/uv/pull/5745
synced_at: 2026-01-12T16:06:59Z
```

# Always strip in release mode

---

_@konstin_

Decreases linux release wheel size by 2MB.

See https://github.com/rust-lang/cargo/issues/14346

Fixes #5683

---

_Label `enhancement` added by @konstin on 2024-08-03 10:39_

---

_@charliermarsh approved on 2024-08-03 11:03_

---

_Merged by @konstin on 2024-08-03 11:11_

---

_Closed by @konstin on 2024-08-03 11:11_

---

_Branch deleted on 2024-08-03 11:11_

---

_Comment by @charliermarsh on 2024-08-03 12:13_

Should we make the same change in Ruff?

---

_Comment by @konstin on 2024-08-03 13:42_

Yes, i expect it'll be some time until a fix in cargo lands on stable.

---
