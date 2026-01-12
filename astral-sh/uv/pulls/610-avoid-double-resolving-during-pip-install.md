```yaml
number: 610
title: "Avoid double-resolving during `pip-install`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pip-install-resolution
created_at: 2023-12-12T01:53:34Z
updated_at: 2023-12-12T17:29:11Z
url: https://github.com/astral-sh/uv/pull/610
synced_at: 2026-01-12T16:04:04Z
```

# Avoid double-resolving during `pip-install`

---

_@charliermarsh_

## Summary

At present, when performing a `pip-install`, we first do a resolution, then take the set of requirements and basically run them through our `pip-sync`, which itself includes re-resolving the dependencies to get a specific `Dist` for each package. (E.g., the set of requirements might say `flask==3.0.0`, but the installer needs a specific _wheel_ or source distribution to install.)

This PR removes this second resolution by exposing the set of pinned packages from the resolution. The main challenge here is that we have an optimization in the resolver such that we let the resolver read metadata from an incompatible wheel as long as a source distribution exists for a given package. This lets us avoid building source distributions in the resolver under the assumption that we'll be able to install the package later on, if needed. As such, the resolver now needs to track the resolution and installation filenames separately.


---

_Review requested from @zanieb by @charliermarsh on 2023-12-12 02:59_

---

_Review requested from @konstin by @charliermarsh on 2023-12-12 02:59_

---

_@zanieb approved on 2023-12-12 04:45_

Looks good to me, but I am less familiar with the changes here.

---

_Review comment by @konstin on `crates/puffin-resolver/src/pins.rs`:11 on 2023-12-12 11:10_

Maybe `FilePins`?

---

_@konstin approved on 2023-12-12 11:11_

---

_Merged by @charliermarsh on 2023-12-12 17:29_

---

_Closed by @charliermarsh on 2023-12-12 17:29_

---

_Branch deleted on 2023-12-12 17:29_

---
