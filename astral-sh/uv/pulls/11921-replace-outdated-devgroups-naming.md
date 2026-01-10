```yaml
number: 11921
title: "Replace outdated DevGroups* naming"
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: main
head: jtfm/rename-dep-groups
created_at: 2025-03-03T10:12:29Z
updated_at: 2025-03-03T15:39:46Z
url: https://github.com/astral-sh/uv/pull/11921
synced_at: 2026-01-10T11:10:39Z
```

# Replace outdated DevGroups* naming

---

_Pull request opened by @jtfmumm on 2025-03-03 10:12_

At certain points in the code, dependency groups are represented by `DevGroups*` naming, probably as a historical artifact. This PR updates the naming.

This includes renaming `uv-configuration/src/dev.rs` to `uv-configuration/src/dependency_groups.rs`.


---

_@Gankra approved on 2025-03-03 14:43_

This is good, but if we're doing renames we could consider further cleanup:

DependencyGroupsSpecification => DependencyGroups
DependencyGroupsManifest => DependencyGroupsWithDefaults

---

_Merged by @jtfmumm on 2025-03-03 15:39_

---

_Closed by @jtfmumm on 2025-03-03 15:39_

---

_Branch deleted on 2025-03-03 15:39_

---
