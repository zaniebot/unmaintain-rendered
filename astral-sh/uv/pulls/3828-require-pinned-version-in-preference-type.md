```yaml
number: 3828
title: "Require pinned version in `Preference` type"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/unnamed
created_at: 2024-05-24T17:56:40Z
updated_at: 2024-05-24T18:48:37Z
url: https://github.com/astral-sh/uv/pull/3828
synced_at: 2026-01-12T16:05:52Z
```

# Require pinned version in `Preference` type

---

_@charliermarsh_

## Summary

This PR makes a variety of invalid states unrepresentable by changing `Preference` to require a `PackageName` and `Version`, rather than accepting a generic `Requirement`. There should be no meaningful behavior changes.


---

_Label `internal` added by @charliermarsh on 2024-05-24 17:56_

---

_@charliermarsh reviewed on 2024-05-24 17:57_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/preferences.rs`:79 on 2024-05-24 17:57_

This is the only possible behavior change. Previously, these were effectively discarded, which seems wrong. (Specifically, we used the URL, but `Preferences` then throws out URL requirements.)

---

_@konstin approved on 2024-05-24 18:13_

---

_Merged by @charliermarsh on 2024-05-24 18:48_

---

_Closed by @charliermarsh on 2024-05-24 18:48_

---

_Branch deleted on 2024-05-24 18:48_

---
