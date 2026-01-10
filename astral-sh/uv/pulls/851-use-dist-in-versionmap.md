```yaml
number: 851
title: "Use `Dist` in `VersionMap`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/use-dist-in-version-map
created_at: 2024-01-09T17:14:26Z
updated_at: 2024-01-09T23:14:44Z
url: https://github.com/astral-sh/uv/pull/851
synced_at: 2026-01-10T15:44:44Z
```

# Use `Dist` in `VersionMap`

---

_Pull request opened by @konstin on 2024-01-09 17:14_

Refactoring split out from find links support: Find links files can be represented as `Dist`, but not really as `File`, they don't have url nor hashes.

`DistRequiresPython` is somewhat odd as an in between type.

---

_@charliermarsh approved on 2024-01-09 17:27_

---

_Label `internal` added by @charliermarsh on 2024-01-09 17:28_

---

_Merged by @konstin on 2024-01-09 23:14_

---

_Closed by @konstin on 2024-01-09 23:14_

---

_Branch deleted on 2024-01-09 23:14_

---
