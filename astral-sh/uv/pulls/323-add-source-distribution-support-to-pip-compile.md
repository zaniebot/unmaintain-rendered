```yaml
number: 323
title: "Add source distribution support to `pip-compile`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/sdist
created_at: 2023-11-06T05:15:34Z
updated_at: 2023-11-06T13:22:38Z
url: https://github.com/astral-sh/uv/pull/323
synced_at: 2026-01-10T15:50:28Z
```

# Add source distribution support to `pip-compile`

---

_Pull request opened by @charliermarsh on 2023-11-06 05:15_

## Summary

This is a first-pass at adding source distribution support to the installer.

The previous installation flow was:

1. Come up with a plan.
1. Find a distribution (specific file) for every package that we'll need to download.
1. Download those distributions.
1. Unzip them (since we assumed they were all wheels).
1. Install them into the virtual environment.

Now, Step (3) downloads both wheels and source distributions, and we insert a step between Steps (3) and (4) to build any source distributions into zipped wheels.

There are a bunch of TODOs, the most important (IMO) is that we basically have two implementations of downloading and building, between the stuff in `puffin_installer` and `puffin_resolver` (namely in `crates/puffin-resolver/src/distribution`). I didn't attempt to clean that up here -- it's already a problem, and it's related to the overall problem we need to solve around unified caching and resource management.

Closes #243.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:208 on 2023-11-06 05:15_

This file is just begging for a nice ASCII diagram.

---

_@charliermarsh reviewed on 2023-11-06 05:15_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-06 05:39_

---

_Review requested from @konstin by @charliermarsh on 2023-11-06 05:39_

---

_@charliermarsh reviewed on 2023-11-06 05:42_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:105 on 2023-11-06 05:42_

The abstractions here overall feel a bit off between `RemoteDistribution` and `Source`. Like it's weird that I'm splitting on `distribution.is_wheel`, then have these two entirely different blocks even though the _type_ of the underlying `distribution` is no different. Room for refinement...

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:149 on 2023-11-06 05:42_

This whole portion of the `if` branch already existed in this file, but Git isn't picking that up. Only the `else` branch is new.

---

_@charliermarsh reviewed on 2023-11-06 05:42_

---

_Label `enhancement` added by @charliermarsh on 2023-11-06 05:44_

---

_@konstin approved on 2023-11-06 09:05_

---

_Merged by @charliermarsh on 2023-11-06 13:22_

---

_Closed by @charliermarsh on 2023-11-06 13:22_

---

_Branch deleted on 2023-11-06 13:22_

---
