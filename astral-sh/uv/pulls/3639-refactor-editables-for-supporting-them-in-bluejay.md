```yaml
number: 3639
title: Refactor editables for supporting them in bluejay commands
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/editables-in-bluejay-commands
created_at: 2024-05-17T15:14:31Z
updated_at: 2024-05-21T09:17:01Z
url: https://github.com/astral-sh/uv/pull/3639
synced_at: 2026-01-12T16:05:46Z
```

# Refactor editables for supporting them in bluejay commands

---

_@konstin_

This is split out from workspaces support, which needs editables in the bluejay commands. It consists mainly of refactorings:

* Move the `editable` module one level up.
* Introduce a `BuiltEditableMetadata` type for `(LocalEditable, Metadata23, Requirements)`.
* Add editables to `InstalledPackagesProvider` so we can use `EmptyInstalledPackages` for them.

---

_Label `preview` added by @konstin on 2024-05-17 15:14_

---

_Comment by @charliermarsh on 2024-05-20 14:19_

This looks good to me. I'll rebase and merge for you.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-05-20 14:19_

---

_@charliermarsh approved on 2024-05-20 16:12_

---

_Merged by @charliermarsh on 2024-05-20 16:22_

---

_Closed by @charliermarsh on 2024-05-20 16:22_

---

_Branch deleted on 2024-05-20 16:22_

---

_@charliermarsh reviewed on 2024-05-20 16:29_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:90 on 2024-05-20 16:29_

@konstin - Slightly weird interaction here, not sure how it should work exactly...

---

_@konstin reviewed on 2024-05-21 09:17_

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:90 on 2024-05-21 09:17_

I thought something like `editable: bool` for path dists in the lockfile? Haven't really dug into the format yet

---
