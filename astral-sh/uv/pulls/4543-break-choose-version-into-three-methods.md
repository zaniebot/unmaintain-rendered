```yaml
number: 4543
title: "Break `choose_version` into three methods"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/choose-version-refactoring
created_at: 2024-06-26T12:23:59Z
updated_at: 2024-06-26T13:15:29Z
url: https://github.com/astral-sh/uv/pull/4543
synced_at: 2026-01-12T16:06:17Z
```

# Break `choose_version` into three methods

---

_@konstin_

`ResolverState::choose_version` had become huge, with an odd match due to the url handling from #4435. This refactoring breaks it into `choose_version`, `choose_version_registry` and `choose_version_url`. No functional changes.


---

_Label `internal` added by @konstin on 2024-06-26 12:23_

---

_@konstin reviewed on 2024-06-26 12:25_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:780 on 2024-06-26 12:25_

It's the same code just dedented, see https://app.semanticdiff.com/gh/astral-sh/uv/pull/4543/changes for a better diff

---

_@charliermarsh approved on 2024-06-26 13:14_

---

_Merged by @konstin on 2024-06-26 13:15_

---

_Closed by @konstin on 2024-06-26 13:15_

---

_Branch deleted on 2024-06-26 13:15_

---
