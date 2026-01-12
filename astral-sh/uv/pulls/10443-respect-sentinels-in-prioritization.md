```yaml
number: 10443
title: Respect sentinels in prioritization
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/sentinel
created_at: 2025-01-09T20:43:37Z
updated_at: 2025-01-09T21:28:00Z
url: https://github.com/astral-sh/uv/pull/10443
synced_at: 2026-01-12T16:09:18Z
```

# Respect sentinels in prioritization

---

_@charliermarsh_

## Summary

If a user provides a constraint like `flask==3.0.0`, that gets expanded to `[3.0.0, 3.0.0+[max])`. So it's not a _singleton_, but it should be treated as such for the purposes of prioritization, since in practice it will almost always map to a single version.


---

_Label `performance` added by @charliermarsh on 2025-01-09 20:43_

---

_Marked ready for review by @charliermarsh on 2025-01-09 20:43_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/version_map.rs`:166 on 2025-01-09 20:44_

It does... Proxy packages use `Ranges::singleton`, which hits this path.

---

_@charliermarsh reviewed on 2025-01-09 20:44_

---

_@zanieb approved on 2025-01-09 20:52_

---

_Merged by @charliermarsh on 2025-01-09 21:19_

---

_Closed by @charliermarsh on 2025-01-09 21:19_

---

_Branch deleted on 2025-01-09 21:19_

---

_@charliermarsh reviewed on 2025-01-09 21:28_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/priority.rs`:183 on 2025-01-09 21:28_

@konstin -- Heads up, I removed this... Otherwise, the `wrong_backtracking_basic` test failed. I think it succeeded by accident before? Since all the conflicts are in singletons.

---
