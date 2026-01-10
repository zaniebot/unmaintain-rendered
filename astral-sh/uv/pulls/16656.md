```yaml
number: 16656
title: Add support for the Simple index API top-level route
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/simple
created_at: 2025-11-09T19:22:40Z
updated_at: 2025-11-10T19:50:20Z
url: https://github.com/astral-sh/uv/pull/16656
synced_at: 2026-01-10T06:28:12Z
```

# Add support for the Simple index API top-level route

---

_Pull request opened by @charliermarsh on 2025-11-09 19:22_

## Summary

At present, we only have support for the detail routes (e.g., `https://pypi.org/simple/requests`), but not the top-level index route (e.g., `https://pypi.org/simple/`). I need this for some downstream work so pulling it into its own PR.


---

_Review requested from @zanieb by @charliermarsh on 2025-11-09 19:22_

---

_Review requested from @konstin by @charliermarsh on 2025-11-09 19:22_

---

_Label `internal` added by @charliermarsh on 2025-11-09 19:22_

---

_Marked ready for review by @charliermarsh on 2025-11-09 19:22_

---

_@zanieb approved on 2025-11-09 19:45_

---

_Review comment by @konstin on `crates/uv-client/src/html.rs`:265 on 2025-11-10 10:58_

What does pip do here, should we check that the href is some form of `/<project>/`, to remove non-project links?

---

_Review comment by @konstin on `crates/uv-client/src/html.rs`:276 on 2025-11-10 10:58_

perf: Should we parse a `PackageName` directly? Depending on the target size, it will only matter if we have all of PyPI or mirroring for some massive monorepo.

---

_Review comment by @konstin on `crates/uv-client/src/html.rs`:1484 on 2025-11-10 11:03_

```suggestion
    /// Test that links without `href` attributes are ignored.
```

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:828 on 2025-11-10 11:09_

Why are we using the uncached client?

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:833 on 2025-11-10 11:12_

Should we show the full path here?

---

_@konstin approved on 2025-11-10 11:16_

---

_Review comment by @charliermarsh on `crates/uv-client/src/html.rs`:265 on 2025-11-10 16:58_

I will check!

---

_@charliermarsh reviewed on 2025-11-10 16:58_

---

_@charliermarsh reviewed on 2025-11-10 16:58_

---

_Review comment by @charliermarsh on `crates/uv-client/src/html.rs`:276 on 2025-11-10 16:58_

Yes sorry about that.

---

_Merged by @charliermarsh on 2025-11-10 19:50_

---

_Closed by @charliermarsh on 2025-11-10 19:50_

---

_Branch deleted on 2025-11-10 19:50_

---
