```yaml
number: 8471
title: "Add support for `default-groups`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: tracking/735
head: charlie/default-groups
created_at: 2024-10-22T17:39:53Z
updated_at: 2024-10-22T19:12:24Z
url: https://github.com/astral-sh/uv/pull/8471
synced_at: 2026-01-10T12:54:10Z
```

# Add support for `default-groups`

---

_Pull request opened by @charliermarsh on 2024-10-22 17:39_

## Summary

This PR adds support for `tool.uv.default-groups`, which defaults to `["dev"]` for backwards-compatibility. These represent the groups we sync by default.


---

_Comment by @charliermarsh on 2024-10-22 17:41_

@zanieb -- We might need `--no-group foo` now that we have this.

---

_Review requested from @zanieb by @charliermarsh on 2024-10-22 17:52_

---

_Label `enhancement` added by @charliermarsh on 2024-10-22 17:52_

---

_@charliermarsh reviewed on 2024-10-22 17:52_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/dev.rs`:239 on 2024-10-22 17:52_

I don't feel strongly on the name here, but this is the specification along with the project-specific defaults.

---

_Comment by @zanieb on 2024-10-22 17:53_

Yeah I was imagining adding `--no-default-groups`, `--no-group` seems fine too.

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/dev.rs`:59 on 2024-10-22 17:54_

I made this the `None` case for `groups: Option<GroupsSpecification>`, to mirror `dev: Option< DevMode>`.

---

_@charliermarsh reviewed on 2024-10-22 17:54_

---

_@zanieb reviewed on 2024-10-22 18:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:587 on 2024-10-22 18:06_

Can we avoid repeating this block in each command?

---

_@zanieb reviewed on 2024-10-22 18:07_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1305 on 2024-10-22 18:07_

Perhaps something like

```suggestion
    error: Default group `bar` (from `tool.uv.default-groups`) is not defined in the project's `dependency-group` table
```

---

_@charliermarsh reviewed on 2024-10-22 18:53_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:587 on 2024-10-22 18:53_

Done

---

_@charliermarsh reviewed on 2024-10-22 18:53_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1305 on 2024-10-22 18:53_

Done

---

_Review requested from @zanieb by @charliermarsh on 2024-10-22 18:57_

---

_@zanieb approved on 2024-10-22 19:03_

---

_Merged by @charliermarsh on 2024-10-22 19:12_

---

_Closed by @charliermarsh on 2024-10-22 19:12_

---

_Branch deleted on 2024-10-22 19:12_

---
