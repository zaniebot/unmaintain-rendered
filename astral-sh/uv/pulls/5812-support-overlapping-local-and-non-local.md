```yaml
number: 5812
title: Support overlapping local and non-local requirements in forks
type: pull_request
state: merged
author: ibraheemdev
labels:
  - bug
  - resolver
assignees: []
merged: true
base: main
head: ibraheem/overlapping-local-base-fork
created_at: 2024-08-06T12:14:17Z
updated_at: 2024-08-06T16:04:38Z
url: https://github.com/astral-sh/uv/pull/5812
synced_at: 2026-01-10T13:31:54Z
```

# Support overlapping local and non-local requirements in forks

---

_Pull request opened by @ibraheemdev on 2024-08-06 12:14_

## Summary

This fixes a bug introduced by https://github.com/astral-sh/uv/pull/5232. It turns out that the `universal_disjoint_base_or_local_requirement` test does not actually do what it was meant to because of the incorrect python requirement. With a valid python requirement, it fails on `main`. The problem is that we try to exclude the original base version from the range of allowed versions to try and prefer local versions. However, in the test, there is a branch that depends on the non-local version, with no applicable local in its fork. We should remove this exclusion as prioritization is handled by the candidate resolver.

---

_Label `bug` added by @ibraheemdev on 2024-08-06 12:14_

---

_Label `resolver` added by @ibraheemdev on 2024-08-06 12:14_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-08-06 12:14_

---

_Renamed from "support overlapping local and non-local requirements in forks" to "Support overlapping local and non-local requirements in forks" by @ibraheemdev on 2024-08-06 12:17_

---

_@charliermarsh approved on 2024-08-06 14:56_

---

_Merged by @ibraheemdev on 2024-08-06 16:04_

---

_Closed by @ibraheemdev on 2024-08-06 16:04_

---

_Branch deleted on 2024-08-06 16:04_

---
