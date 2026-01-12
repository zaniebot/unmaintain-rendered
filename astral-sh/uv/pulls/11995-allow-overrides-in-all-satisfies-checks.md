```yaml
number: 11995
title: Allow overrides in all satisfies checks
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/overrides-ii
created_at: 2025-03-06T01:51:43Z
updated_at: 2025-03-06T02:54:21Z
url: https://github.com/astral-sh/uv/pull/11995
synced_at: 2026-01-12T16:10:05Z
```

# Allow overrides in all satisfies checks

---

_@charliermarsh_

## Summary

This PR adds support for `SitePackages::satisfies` with unnamed overrides and requirements.

The main challenge here was cases like: you have a `requirements.in` with `git+https://github.com/pallets/flask` in it, and an `overrides.txt` with `flask==2.0.0` in it. You _need_ to include `flask==2.0.0`, but you can't know that without resolving the unnamed URL requirement (since overrides only take effect when the package is included, like constraints).

We now make the assumption that any unnamed overrides _are_ relevant, for the purpose of the satisfies check. This is conservative, but this whole check is an optimization anyway.


---

_Label `performance` added by @charliermarsh on 2025-03-06 01:51_

---

_@charliermarsh reviewed on 2025-03-06 01:52_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:376 on 2025-03-06 01:52_

The strategy here is: map each unnamed requirement to a named requirement. Then use the "everything must have names" version of this method.

---

_Merged by @charliermarsh on 2025-03-06 02:54_

---

_Closed by @charliermarsh on 2025-03-06 02:54_

---

_Branch deleted on 2025-03-06 02:54_

---
