```yaml
number: 3458
title: "Reduce `GitSourceDist` URL usage"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/git-source-dist-url
created_at: 2024-05-08T13:06:18Z
updated_at: 2024-05-14T08:59:39Z
url: https://github.com/astral-sh/uv/pull/3458
synced_at: 2026-01-12T16:05:39Z
```

# Reduce `GitSourceDist` URL usage

---

_@konstin_

Refactor the easy-to-remove usage of the `VerbatimUrl` on `GitSourceDist`

---

_Marked ready for review by @konstin on 2024-05-08 13:41_

---

_Label `internal` added by @konstin on 2024-05-08 13:42_

---

_@charliermarsh reviewed on 2024-05-14 01:55_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/redirect.rs`:51 on 2024-05-14 01:55_

@konstin - This actually looks like an import TODO.

---

_@charliermarsh reviewed on 2024-05-14 01:56_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/redirect.rs`:51 on 2024-05-14 01:56_

Where we losing this info?

---

_Renamed from "Reduce GitSourceDist url usage" to "Reduce `GitSourceDist` URL usage" by @charliermarsh on 2024-05-14 01:59_

---

_Merged by @charliermarsh on 2024-05-14 02:03_

---

_Closed by @charliermarsh on 2024-05-14 02:03_

---

_Branch deleted on 2024-05-14 02:03_

---

_@konstin reviewed on 2024-05-14 08:59_

---

_Review comment by @konstin on `crates/uv-resolver/src/redirect.rs`:51 on 2024-05-14 08:59_

The bottleneck is `GitSourceUrl` -> `resolve_precise`

---
