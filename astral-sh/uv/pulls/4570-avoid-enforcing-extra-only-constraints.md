```yaml
number: 4570
title: Avoid enforcing extra-only constraints
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/c
created_at: 2024-06-26T22:39:13Z
updated_at: 2024-06-26T22:52:48Z
url: https://github.com/astral-sh/uv/pull/4570
synced_at: 2026-01-12T16:06:19Z
```

# Avoid enforcing extra-only constraints

---

_@charliermarsh_

## Summary

In the dependency refactor (https://github.com/astral-sh/uv/pull/4430), the logic for requirements and constraints was combined. Specifically, we were applying constraints _before_ filtering on markers and extras, and then applying that same filtering to the constraints. As a result, constraints that should only be activated when an extra is enabled were being enabled unconditionally.

Closes https://github.com/astral-sh/uv/issues/4569.


---

_Label `bug` added by @charliermarsh on 2024-06-26 22:39_

---

_@charliermarsh reviewed on 2024-06-26 22:39_

---

_Review comment by @charliermarsh on `req.txt`:3 on 2024-06-26 22:39_

Sorry, I must've checked this in at some point accidentally.

---

_@charliermarsh reviewed on 2024-06-26 22:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1385 on 2024-06-26 22:40_

This is almost the same as above but not quite (the filtering on extras is different), just as it was prior to the refactor (intentionally so).

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1313 on 2024-06-26 22:40_

The key is that we shouldn't apply the constraints here; we only apply them _after_ we've filtered requirements.

---

_@charliermarsh reviewed on 2024-06-26 22:40_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-26 22:40_

---

_Review requested from @konstin by @charliermarsh on 2024-06-26 22:40_

---

_Comment by @charliermarsh on 2024-06-26 22:43_

Link to the problematic change: https://github.com/astral-sh/uv/pull/4430/files#diff-6be6d80fe4821c47b70a372260f55e73b8da8182b8dcad7525d5cd3eb584532bR1285

Compared to before, when we only applied constraints _after_ filtering requirements: https://github.com/astral-sh/uv/pull/4430/files#diff-628347083e870ac5a60372227e35df821298fa35b830ae22517ad2ea7ad1c459L200


---

_@zanieb approved on 2024-06-26 22:49_

Thanks for pulling up the additional context for me!

---

_Merged by @charliermarsh on 2024-06-26 22:52_

---

_Closed by @charliermarsh on 2024-06-26 22:52_

---

_Branch deleted on 2024-06-26 22:52_

---
