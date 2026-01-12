```yaml
number: 6762
title: "Support `{package}@{version}` in `uv tool install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/at
created_at: 2024-08-28T15:52:08Z
updated_at: 2024-08-28T16:44:13Z
url: https://github.com/astral-sh/uv/pull/6762
synced_at: 2026-01-12T16:07:31Z
```

# Support `{package}@{version}` in `uv tool install`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/6759.

Closes https://github.com/astral-sh/uv/issues/6535.


---

_Label `enhancement` added by @charliermarsh on 2024-08-28 15:52_

---

_Label `cli` added by @charliermarsh on 2024-08-28 15:52_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-28 16:11_

---

_Marked ready for review by @charliermarsh on 2024-08-28 16:11_

---

_@zanieb reviewed on 2024-08-28 16:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:87 on 2024-08-28 16:13_

Refresh all? Or just refresh Ruff?

---

_@charliermarsh reviewed on 2024-08-28 16:15_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:87 on 2024-08-28 16:15_

In `uv tool run` at least we refresh everything. I think we might have to refresh everything here because we don't know the package name yet...?

---

_@charliermarsh reviewed on 2024-08-28 16:16_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:212 on 2024-08-28 16:16_

This part here doesn't exist in `uv tool run`, because there, we always resolve an ephemeral environment and recreate it if any dependencies changed. Here, we need to ensure that we upgrade when re-resolving the tool environment.

---

_@zanieb reviewed on 2024-08-28 16:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:87 on 2024-08-28 16:17_

Okay. Seems fine.

---

_@zanieb reviewed on 2024-08-28 16:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/mod.rs`:76 on 2024-08-28 16:19_

Does this collide with #6683?

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/mod.rs`:76 on 2024-08-28 16:20_

I was hoping to merge that today too. Unsure if it's 100% right even though Konsti tried it?

---

_@zanieb reviewed on 2024-08-28 16:20_

---

_@zanieb approved on 2024-08-28 16:22_

Could probably use documentation in https://docs.astral.sh/uv/concepts/tools/#tool-versions

---

_Merged by @charliermarsh on 2024-08-28 16:40_

---

_Closed by @charliermarsh on 2024-08-28 16:40_

---

_Branch deleted on 2024-08-28 16:40_

---

_Comment by @charliermarsh on 2024-08-28 16:40_

That Windows failure looks unrelated.

---

_Comment by @charliermarsh on 2024-08-28 16:41_

Something in the workspace tests.

---

_Comment by @zanieb on 2024-08-28 16:44_

> Something in the workspace tests.

Saw that flake earlier too

---
