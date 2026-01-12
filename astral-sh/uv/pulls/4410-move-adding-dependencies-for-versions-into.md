```yaml
number: 4410
title: Move adding dependencies for versions into dedicated method
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/refactor-adding-versions
created_at: 2024-06-19T11:13:54Z
updated_at: 2024-06-19T18:19:13Z
url: https://github.com/astral-sh/uv/pull/4410
synced_at: 2026-01-12T16:06:12Z
```

# Move adding dependencies for versions into dedicated method

---

_@konstin_

To support diverging urls, we have to check urls when adding dependencies (after forking). To prepare for this, i've moved adding dependencies for the current version to `SolveState::add_package_version_dependencies` and removed the duplication when checking for self-dependencies.

This changed is joined with a change in pubgrub (https://github.com/astral-sh/pubgrub/pull/27) that simplifies the same code path.


---

_Label `internal` added by @konstin on 2024-06-19 11:13_

---

_Review requested from @BurntSushi by @konstin on 2024-06-19 15:52_

---

_@BurntSushi approved on 2024-06-19 16:08_

Nice, this is great, thank you!

---

_Merged by @konstin on 2024-06-19 18:19_

---

_Closed by @konstin on 2024-06-19 18:19_

---

_Branch deleted on 2024-06-19 18:19_

---
