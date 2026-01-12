```yaml
number: 15708
title: "Allow `editable = false` for workspace sources"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/non-editable
created_at: 2025-09-06T16:16:43Z
updated_at: 2025-09-07T15:41:18Z
url: https://github.com/astral-sh/uv/pull/15708
synced_at: 2026-01-12T16:11:53Z
```

# Allow `editable = false` for workspace sources

---

_@charliermarsh_

## Summary

This ended up being a bit more complex, similar to `package = false`, because we need to understand the editable status _globally_ across the workspace based on the packages that depend on it.

Closes https://github.com/astral-sh/uv/issues/15686.


---

_Label `enhancement` added by @charliermarsh on 2025-09-06 16:17_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-06 18:40_

---

_Review requested from @konstin by @charliermarsh on 2025-09-06 18:40_

---

_Marked ready for review by @charliermarsh on 2025-09-06 18:40_

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:430 on 2025-09-07 11:55_

nit: we don't need the entry API.

```rust
                let existing = required_members.insert(package.clone(), *editable);
                if let Some(Some(existing)) = existing {
                    if let Some(editable) = editable {
                        // If there are conflicting `editable` values, raise an error.
                        if existing != *editable {
                            return Err(WorkspaceError::EditableConflict(package.clone()));
                        }
                    }
                }
```

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:480 on 2025-09-07 11:57_

nit: Can we give this a clearer name, to make `member.pyproject_toml().is_package(status.is_none())` clearer?

---

_@konstin approved on 2025-09-07 11:57_

---

_@charliermarsh reviewed on 2025-09-07 15:13_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:480 on 2025-09-07 15:13_

Yeah I'm gonna make it an enum instead of `Option<bool>` to make this clearer.

---

_Merged by @charliermarsh on 2025-09-07 15:41_

---

_Closed by @charliermarsh on 2025-09-07 15:41_

---

_Branch deleted on 2025-09-07 15:41_

---
