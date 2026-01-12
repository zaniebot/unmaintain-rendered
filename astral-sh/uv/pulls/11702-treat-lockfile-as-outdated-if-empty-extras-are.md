```yaml
number: 11702
title: Treat lockfile as outdated if (empty) extras are added
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/p-7
created_at: 2025-02-22T00:42:18Z
updated_at: 2025-02-22T04:51:06Z
url: https://github.com/astral-sh/uv/pull/11702
synced_at: 2026-01-12T16:09:57Z
```

# Treat lockfile as outdated if (empty) extras are added

---

_@charliermarsh_

## Summary

Now that we track extras in the lockfile, we should validate them in `--locked`.


---

_Label `bug` added by @charliermarsh on 2025-02-22 00:42_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:1057 on 2025-02-22 02:03_

(1, 0), not (1, 1)? Oh because it's querying for requires-dist and not provides-extras? (I take it from this there was a v0 lockfile at some point...)

Might want to make this a method with a comprehensible name if it's getting copied so many places. (Or just leave comments for why each version range is chosen)

---

_@Gankra approved on 2025-02-22 02:31_

---

_Merged by @charliermarsh on 2025-02-22 04:51_

---

_Closed by @charliermarsh on 2025-02-22 04:51_

---

_Branch deleted on 2025-02-22 04:51_

---
