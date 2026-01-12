```yaml
number: 6566
title: "Use relative paths for `--find-links` and local registries"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/find-links-local
created_at: 2024-08-24T03:25:43Z
updated_at: 2024-08-26T00:14:54Z
url: https://github.com/astral-sh/uv/pull/6566
synced_at: 2026-01-12T16:07:25Z
```

# Use relative paths for `--find-links` and local registries

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/uv/issues/6458


---

_Label `bug` added by @charliermarsh on 2024-08-24 03:25_

---

_Review requested from @konstin by @charliermarsh on 2024-08-24 18:05_

---

_@charliermarsh reviewed on 2024-08-24 18:06_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:2309 on 2024-08-24 18:06_

Not _really_ that happy with this, but I didn't want to introduce separate enum variants for remote vs. local registries.

---

_Comment by @charliermarsh on 2024-08-24 23:54_

I think the last piece here is that I need to test on Windows with files on different drives.

---

_Comment by @charliermarsh on 2024-08-25 02:34_

Ok, we need to fall back to absolute in those cases.

---

_Comment by @charliermarsh on 2024-08-25 02:35_

Tested and confirmed on Windows.

---

_Merged by @charliermarsh on 2024-08-25 02:41_

---

_Closed by @charliermarsh on 2024-08-25 02:41_

---

_Branch deleted on 2024-08-25 02:41_

---

_Review comment by @konstin on `crates/distribution-types/src/file.rs`:171 on 2024-08-25 17:17_

Are these `serde(transparent)`, or am i missing something?

---

_@konstin reviewed on 2024-08-25 17:24_

---

_@charliermarsh reviewed on 2024-08-26 00:14_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:171 on 2024-08-26 00:14_

Yes thank you! https://github.com/astral-sh/uv/pull/6633

---
