```yaml
number: 930
title: Share flat index across resolutions
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/flat-index
created_at: 2024-01-15T15:55:16Z
updated_at: 2024-01-15T16:05:02Z
url: https://github.com/astral-sh/uv/pull/930
synced_at: 2026-01-12T16:04:18Z
```

# Share flat index across resolutions

---

_@charliermarsh_

## Summary

This PR restructures the flat index fetching in a few ways:

1. It now lives in its own `FlatIndexClient`, since it felt a bit awkward (in my opinion) for it to live in `RegistryClient`.
2. We now fetch the `FlatIndex` outside of the resolver. This has a few benefits: (1) the resolver construct is no longer `async` and no longer returns `Result`, which feels better for a resolver; and (2) we can share the `FlatIndex` across resolutions rather than re-fetching it for every source distribution build.

---

_Review requested from @konstin by @charliermarsh on 2024-01-15 15:55_

---

_Label `internal` added by @charliermarsh on 2024-01-15 15:55_

---

_@charliermarsh reviewed on 2024-01-15 15:58_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/flat_index.rs`:169 on 2024-01-15 15:58_

All just moved over from `RegistryClient`.

---

_Review comment by @konstin on `crates/distribution-types/src/index_url.rs`:139 on 2024-01-15 15:59_

Isn't that where we want to allow flat indexes? Or is that a separate change? pip:

> ```
>   --no-index                  Ignore package index (only looking at --find-links URLs instead).
> ```

---

_@konstin approved on 2024-01-15 16:01_

---

_@charliermarsh reviewed on 2024-01-15 16:01_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index_url.rs`:139 on 2024-01-15 16:01_

Do we want to allow flat-indexes? I wasn't sure honestly.

---

_Merged by @charliermarsh on 2024-01-15 16:02_

---

_Closed by @charliermarsh on 2024-01-15 16:02_

---

_Branch deleted on 2024-01-15 16:02_

---

_@charliermarsh reviewed on 2024-01-15 16:02_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index_url.rs`:139 on 2024-01-15 16:02_

We probably should. We might want an `--offline` flag for "don't do any fetches".

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index_url.rs`:139 on 2024-01-15 16:05_

Oh, this validation actually happens via Clap. I'll remove this comment.

---

_@charliermarsh reviewed on 2024-01-15 16:05_

---
