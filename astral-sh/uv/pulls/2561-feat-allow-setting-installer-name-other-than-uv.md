```yaml
number: 2561
title: "feat: allow setting installer name other than `uv`"
type: pull_request
state: merged
author: wolfv
labels: []
assignees: []
merged: true
base: main
head: set-installer-name
created_at: 2024-03-20T10:19:05Z
updated_at: 2024-03-20T14:51:38Z
url: https://github.com/astral-sh/uv/pull/2561
synced_at: 2026-01-12T16:05:06Z
```

# feat: allow setting installer name other than `uv`

---

_@wolfv_

## Summary

We would like to be able to configure the installer-name so that other tools can co-exist with `uv`. In `pixi` we would like to use `pixi-uv` as the installer name, for example, to be able to distinguish them from packages installed by pure `uv`.

---

_Review comment by @charliermarsh on `crates/uv-installer/src/installer.rs`:43 on 2024-03-20 14:02_

Do you mind if I change this to `Option<String>` to make it clear that this needs owned data and avoid the hidden clone?

---

_@charliermarsh reviewed on 2024-03-20 14:02_

---

_@wolfv reviewed on 2024-03-20 14:06_

---

_Review comment by @wolfv on `crates/uv-installer/src/installer.rs`:43 on 2024-03-20 14:06_

Don't mind it at all.

---

_@charliermarsh approved on 2024-03-20 14:11_

Thx!

---

_Merged by @charliermarsh on 2024-03-20 14:36_

---

_Closed by @charliermarsh on 2024-03-20 14:36_

---

_Branch deleted on 2024-03-20 14:51_

---

_Branch restored on 2024-03-20 14:51_

---

_Branch deleted on 2024-03-20 14:51_

---
