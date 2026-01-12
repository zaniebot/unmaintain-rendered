```yaml
number: 616
title: "Towards using `prepare_metadata_for_build_wheel` in the resolver"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: source-build-trait
created_at: 2023-12-12T13:00:09Z
updated_at: 2023-12-12T20:45:38Z
url: https://github.com/astral-sh/uv/pull/616
synced_at: 2026-01-12T16:04:04Z
```

# Towards using `prepare_metadata_for_build_wheel` in the resolver

---

_@konstin_

Make `prepare_metadata_for_build_wheel` accessible across the puffin codebase by splitting the built call into a setup, a metadata and a wheel call. This does not actually use the hook yet, but it's the required refactoring for it.

Part of #599.

---

_@konstin reviewed on 2023-12-12 13:01_

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:559 on 2023-12-12 13:01_

I chose this name because this is only an indirection to make cargo happy with no life on its own.

---

_@konstin reviewed on 2023-12-12 13:03_

---

_Review comment by @konstin on `crates/puffin-distribution/src/source_dist.rs`:649 on 2023-12-12 13:03_

This is the relevant call, it's currently locked inside the expectation that we want a wheel that gets cached and not just metadata. But one step at a time.

---

_Review requested from @charliermarsh by @konstin on 2023-12-12 13:03_

---

_@charliermarsh approved on 2023-12-12 20:16_

---

_Merged by @charliermarsh on 2023-12-12 20:45_

---

_Closed by @charliermarsh on 2023-12-12 20:45_

---

_Branch deleted on 2023-12-12 20:45_

---
