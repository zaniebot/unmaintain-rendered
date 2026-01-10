```yaml
number: 10024
title: Stable order for virtual packages
type: pull_request
state: merged
author: konstin
labels:
  - resolver
assignees: []
merged: true
base: main
head: konsti/pubgrub-priorities
created_at: 2024-12-19T10:04:18Z
updated_at: 2024-12-20T09:28:47Z
url: https://github.com/astral-sh/uv/pull/10024
synced_at: 2026-01-10T12:00:01Z
```

# Stable order for virtual packages

---

_Pull request opened by @konstin on 2024-12-19 10:04_

uv gives priorities to packages by package name, not by virtual package (`PubGrubPackage`). pubgrub otoh when prioritizing order the virtual packages. When the order of virtual packages changes, uv changes its resolutions and error messages. This means uv was depending on implementation details of pubgrub's prioritization caching.

This broke with https://github.com/pubgrub-rs/pubgrub/pull/299, which added a tiebreaker term that made pubgrub's sorting deterministic given a deterministic ordering of allocating the packages (which happens the first time pubgrub sees a package).

The new custom tiebreaker decreases the difference to upstream pubgrub.

---

_Label `resolver` added by @konstin on 2024-12-19 10:04_

---

_Comment by @konstin on 2024-12-19 15:26_

For transformers, the difference comes from `fsspec` and `datasets` being mutually incompatible. Using just the two packages as input we can coerce both versions, which now appear in different, hence the slightly larger lockfile.

```diff
8,9c8,9
< datasets==2.20.0
< dill==0.3.8
---
> datasets==2.14.4
> dill==0.3.7
12c12
< fsspec==2024.5.0
---
> fsspec==2024.6.1
16c16
< multiprocess==0.70.16
---
> multiprocess==0.70.15
21d20
< pyarrow-hotfix==0.6
```

---

_Marked ready for review by @konstin on 2024-12-19 15:26_

---

_@charliermarsh approved on 2024-12-19 16:46_

---

_Merged by @konstin on 2024-12-20 09:28_

---

_Closed by @konstin on 2024-12-20 09:28_

---

_Branch deleted on 2024-12-20 09:28_

---
