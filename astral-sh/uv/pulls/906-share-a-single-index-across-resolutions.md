```yaml
number: 906
title: "Share a single `Index` across resolutions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/share
created_at: 2024-01-13T03:45:49Z
updated_at: 2024-01-16T05:37:16Z
url: https://github.com/astral-sh/uv/pull/906
synced_at: 2026-01-10T15:39:02Z
```

# Share a single `Index` across resolutions

---

_Pull request opened by @charliermarsh on 2024-01-13 03:45_

## Summary

This PR uses a single `Index` that's shared between the top-level resolver and any sub-resolutions happen in the course of that top-level resolution (namely, to resolve build dependencies for any source distributions).

In theory it's an optimization, since (e.g.) if we have two packages that both need the `flit-core` build system, and we attempt to build them both at once, we'll only fetch its metadata _once_, and share it across the two resolutions. In practice, I haven't been able to get this to show up in benchmarks. I suspect you'd need a _lot_ of source distributions for it to matter... Though it may still be worth doing, it strikes me as a cleaner design.

Closes #200.

Closes #541.

---

_Review requested from @konstin by @charliermarsh on 2024-01-13 03:45_

---

_Label `internal` added by @charliermarsh on 2024-01-13 03:55_

---

_@zanieb approved on 2024-01-13 04:42_

---

_Review comment by @konstin on `crates/distribution-types/src/simple_metadata.rs`:50 on 2024-01-14 09:47_

```suggestion
                metadata
                    .0
                    .entry(version.clone())
                    .or_default()
                    .push(filename, file);
```

---

_@konstin approved on 2024-01-14 09:49_

---

_Merged by @charliermarsh on 2024-01-16 05:37_

---

_Closed by @charliermarsh on 2024-01-16 05:37_

---

_Branch deleted on 2024-01-16 05:37_

---
