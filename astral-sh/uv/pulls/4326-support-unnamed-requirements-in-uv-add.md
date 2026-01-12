```yaml
number: 4326
title: "Support unnamed requirements in `uv add`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: ibraheem/uv-add-unnamed
created_at: 2024-06-14T13:56:51Z
updated_at: 2024-06-14T17:42:40Z
url: https://github.com/astral-sh/uv/pull/4326
synced_at: 2026-01-12T16:06:09Z
```

# Support unnamed requirements in `uv add`

---

_@ibraheemdev_

## Summary

Support unnamed URL requirements in `uv add`. For example, `uv add git+https://github.com/pallets/flask`.

Part of https://github.com/astral-sh/uv/issues/3959.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-14 15:49_

---

_Review requested from @zanieb by @ibraheemdev on 2024-06-14 17:00_

---

_@zanieb reviewed on 2024-06-14 17:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:42 on 2024-06-14 17:16_

Note we should only discover an interpreter not create an environment if syncing is disabled (looks like there's no toggle for that yet)

---

_@zanieb reviewed on 2024-06-14 17:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:66 on 2024-06-14 17:20_

Created #4330 to track the duplication of this after declaring a `BaseClientBuilder` above.

---

_@zanieb reviewed on 2024-06-14 17:22_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:370 on 2024-06-14 17:22_

Since `add` performs a sync shouldn't it receive `ResolverInstallerSettings`?

---

_@zanieb reviewed on 2024-06-14 17:22_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:370 on 2024-06-14 17:22_

Partially unrelated to your work here. cc @charliermarsh 

---

_@zanieb approved on 2024-06-14 17:23_

---

_Label `preview` added by @zanieb on 2024-06-14 17:23_

---

_@ibraheemdev reviewed on 2024-06-14 17:34_

---

_Review comment by @ibraheemdev on `crates/uv/src/settings.rs`:370 on 2024-06-14 17:34_

Oh yes, I think it should. 

---

_Merged by @ibraheemdev on 2024-06-14 17:42_

---

_Closed by @ibraheemdev on 2024-06-14 17:42_

---

_Branch deleted on 2024-06-14 17:42_

---
