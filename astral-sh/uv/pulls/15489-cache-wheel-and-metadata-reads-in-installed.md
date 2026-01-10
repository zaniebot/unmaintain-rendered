```yaml
number: 15489
title: "Cache `WHEEL` and `METADATA` reads in installed distributions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/once-lock
created_at: 2025-08-24T18:56:46Z
updated_at: 2025-08-25T13:40:21Z
url: https://github.com/astral-sh/uv/pull/15489
synced_at: 2026-01-10T06:44:33Z
```

# Cache `WHEEL` and `METADATA` reads in installed distributions

---

_Pull request opened by @charliermarsh on 2025-08-24 18:56_

## Summary

Uses interior mutability to cache the reads. This follows the pattern we use for reading the platform tags in `Interpreter::tags`.


---

_Label `performance` added by @charliermarsh on 2025-08-24 18:56_

---

_Review requested from @konstin by @charliermarsh on 2025-08-24 18:56_

---

_Marked ready for review by @charliermarsh on 2025-08-24 18:56_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/installed.rs`:77 on 2025-08-25 10:21_

Usually, I prefer centralized caches for non-pure data, it took me a bit to realize that this works as `InstalledDist` gets invalidated anyway if we modify the environment.

---

_@konstin approved on 2025-08-25 10:21_

---

_@konstin reviewed on 2025-08-25 12:25_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/installed.rs`:77 on 2025-08-25 12:25_

I think we should leave an inline comment about that, to make this connection clear to readers in the future.

---

_Merged by @charliermarsh on 2025-08-25 13:40_

---

_Closed by @charliermarsh on 2025-08-25 13:40_

---

_Branch deleted on 2025-08-25 13:40_

---
