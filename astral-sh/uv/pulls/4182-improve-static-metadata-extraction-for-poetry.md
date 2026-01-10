```yaml
number: 4182
title: Improve static metadata extraction for Poetry projects
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/err
created_at: 2024-06-10T00:31:07Z
updated_at: 2024-06-10T16:35:53Z
url: https://github.com/astral-sh/uv/pull/4182
synced_at: 2026-01-10T13:54:02Z
```

# Improve static metadata extraction for Poetry projects

---

_Pull request opened by @charliermarsh on 2024-06-10 00:31_

## Summary

Adds handling for a few cases to improve interoperability with Poetry:

- If the `project` schema is invalid, we now raise a hard error, rather than treating the metadata as dynamic and then falling back to the build backend. This could cause problems, I'm not sure. It's stricter than before.
- If the project contains `tool.poetry` but omits `project.dependencies`, we now treat it as dynamic. We could go even further and treat _any_ Poetry project as dynamic, but then we'd be ignoring user-declared dependencies, which is also confusing.

Closes https://github.com/astral-sh/uv/issues/4142.


---

_Label `error messages` added by @charliermarsh on 2024-06-10 00:31_

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:217 on 2024-06-10 09:46_

We should warn, that's a spec violation

---

_@konstin approved on 2024-06-10 09:47_

---

_@charliermarsh reviewed on 2024-06-10 12:16_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:217 on 2024-06-10 12:16_

It's a spec violation in _Poetry_, right? What the user wrote is totally fine.

---

_Merged by @charliermarsh on 2024-06-10 14:26_

---

_Closed by @charliermarsh on 2024-06-10 14:26_

---

_Branch deleted on 2024-06-10 14:26_

---

_@charliermarsh reviewed on 2024-06-10 14:26_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata.rs`:217 on 2024-06-10 14:26_

Merging for now but will follow-up based on your reply.

---

_@konstin reviewed on 2024-06-10 16:35_

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:217 on 2024-06-10 16:35_

We should warn because using poetry <2 and `[project]` together without setting `dynamic` has unintended consequences, either on their own is technically compliant

---
