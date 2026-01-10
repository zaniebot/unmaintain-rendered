```yaml
number: 8892
title: "Add `--all-groups` to `uv sync|run|export|tree`"
type: pull_request
state: merged
author: j178
labels:
  - enhancement
assignees: []
merged: true
base: main
head: all-groups
created_at: 2024-11-07T16:59:47Z
updated_at: 2024-11-20T16:33:42Z
url: https://github.com/astral-sh/uv/pull/8892
synced_at: 2026-01-10T12:00:00Z
```

# Add `--all-groups` to `uv sync|run|export|tree`

---

_Pull request opened by @j178 on 2024-11-07 16:59_

## Summary

Closes #8594

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:81 on 2024-11-07 17:35_

cc @charliermarsh

Just want to confirm you think this is the right structure. The parent type differs pretty significantly from `ExtrasSpecification`.

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:69 on 2024-11-07 17:35_

I would probably call this `IncludeGroups` for clarity

---

_@zanieb reviewed on 2024-11-07 17:35_

Exciting :)

---

_Review requested from @charliermarsh by @zanieb on 2024-11-07 17:35_

---

_Comment by @j178 on 2024-11-07 17:46_

`uv sync --frozen --all-groups` could panic now, gonna check it tomorrow.

---

_@j178 reviewed on 2024-11-08 16:46_

---

_Review comment by @j178 on `crates/uv-resolver/src/lock/mod.rs`:636 on 2024-11-08 16:46_

I'm struggling to find a straightforward way to make `dev.iter()` work with `IncludeGroups::All`, as it needs to be resolved to specific groups before iteration. Consequently, I changed `dev.iter()` to `dev.contains(group)`, allowing `DevGroupManifest` to decide which groups are included. Hopefully, there is a more elegant solution than this.

---

_Marked ready for review by @j178 on 2024-11-08 16:47_

---

_Comment by @charliermarsh on 2024-11-20 03:59_

I'll review tomorrow -- sorry for the delay.

---

_Comment by @charliermarsh on 2024-11-20 15:01_

This generally looks good. I have some ideas for addressing the comment you left around `dev.iter()`, etc. I'll try to merge this today.

---

_Label `enhancement` added by @charliermarsh on 2024-11-20 15:01_

---

_@charliermarsh reviewed on 2024-11-20 15:28_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/tree.rs`:66 on 2024-11-20 15:28_

Why did this move @j178?

---

_@j178 reviewed on 2024-11-20 15:34_

---

_Review comment by @j178 on `crates/uv/src/commands/project/tree.rs`:66 on 2024-11-20 15:34_

by accident I think...

---

_@charliermarsh approved on 2024-11-20 15:58_

---

_Merged by @charliermarsh on 2024-11-20 16:07_

---

_Closed by @charliermarsh on 2024-11-20 16:07_

---

_Branch deleted on 2024-11-20 16:33_

---
