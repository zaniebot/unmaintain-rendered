```yaml
number: 5930
title: "Improve the `uv sync` CLI documentation"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/sync-docs
created_at: 2024-08-08T20:48:23Z
updated_at: 2024-08-09T19:46:34Z
url: https://github.com/astral-sh/uv/pull/5930
synced_at: 2026-01-10T13:31:54Z
```

# Improve the `uv sync` CLI documentation

---

_Pull request opened by @zanieb on 2024-08-08 20:48_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-08-08 20:48_

---

_Label `cli` added by @zanieb on 2024-08-08 20:48_

---

_Label `preview` added by @zanieb on 2024-08-08 20:48_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-08 22:00_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:571 on 2024-08-09 01:04_

"project dependencies" instead of "dependencies of the project"

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:572 on 2024-08-09 01:04_

"up-to-date"

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:597 on 2024-08-09 01:04_

"flags are"?

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2224 on 2024-08-09 01:05_

I feel like this got removed from some other commands in previous PRs?

---

_@charliermarsh approved on 2024-08-09 01:05_

---

_@zanieb reviewed on 2024-08-09 19:19_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:571 on 2024-08-09 19:19_

```suggestion
    /// Syncing ensures that all project dependencies are installed and
```

---

_@zanieb reviewed on 2024-08-09 19:19_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:572 on 2024-08-09 19:19_

```suggestion
    /// up-to-date with the lockfile. Syncing also removes packages that are not
```

---

_@zanieb reviewed on 2024-08-09 19:21_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:597 on 2024-08-09 19:21_

Only one can be provided, idk e.g. it shortens to "unless the `--locked` flag is provided". Could go either way, don't know what is "correct".

---

_@zanieb reviewed on 2024-08-09 19:21_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2224 on 2024-08-09 19:21_

I've usually moved it out of the first line but retained it in general. I can audit again at the end.

---

_Merged by @zanieb on 2024-08-09 19:46_

---

_Closed by @zanieb on 2024-08-09 19:46_

---

_Branch deleted on 2024-08-09 19:46_

---
