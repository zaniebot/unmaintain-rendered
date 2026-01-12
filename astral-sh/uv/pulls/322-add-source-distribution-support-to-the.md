```yaml
number: 322
title: "Add source distribution support to the `DistributionFinder`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/select
created_at: 2023-11-06T04:48:45Z
updated_at: 2023-11-06T05:16:06Z
url: https://github.com/astral-sh/uv/pull/322
synced_at: 2026-01-12T16:03:52Z
```

# Add source distribution support to the `DistributionFinder`

---

_@charliermarsh_

## Summary

This just enables the `DistributionFinder` (previously known as the `WheelFinder`) to select source distributions when there are no matching wheels for a given platform. As a reminder, the `DistributionFinder` is a simple resolver that doesn't look at any dependencies: it just takes a set of pinned packages, and finds a distribution to install to satisfy each requirement.

---

_Merged by @charliermarsh on 2023-11-06 05:16_

---

_Closed by @charliermarsh on 2023-11-06 05:16_

---

_Branch deleted on 2023-11-06 05:16_

---
