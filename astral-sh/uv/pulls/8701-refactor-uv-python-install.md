```yaml
number: 8701
title: "Refactor `uv python install`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/refactor-inst
created_at: 2024-10-30T15:34:18Z
updated_at: 2024-10-30T21:28:09Z
url: https://github.com/astral-sh/uv/pull/8701
synced_at: 2026-01-10T12:54:15Z
```

# Refactor `uv python install`

---

_Pull request opened by @zanieb on 2024-10-30 15:34_

Pulling out of https://github.com/astral-sh/uv/pull/8650 for readability.

Trying to clean this up to simplify extensions in the future. This is not a strict refactor, there are behavioral changes here. 

- Adds some structs for managing state.
- Addresses some likely inconsistent behavior for weird edge-cases. 
    - We fill platform information before checking if a request is satisfied.
    - We error earlier if we can't find a download for the request, i.e., even if you somehow have it installed.
    - Only reports versions as uninstalled if a download actually replaces them.
- Moves some of the default output to tracing messages.
- Even if an installation was already satisfied, we'll check that it is setup properly

---

_Label `internal` added by @zanieb on 2024-10-30 15:34_

---

_Marked ready for review by @zanieb on 2024-10-30 16:55_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:85 on 2024-10-30 20:13_

I think you can just `#[derive(Default)]` for `Changelog` which should give you this automatically without specifying the members. You can also have a `fn new()` that just calls `Changelog::default` if you want. (I'd add `Debug` too personally.)

---

_@charliermarsh reviewed on 2024-10-30 20:13_

---

_@charliermarsh approved on 2024-10-30 20:14_

---

_@zanieb reviewed on 2024-10-30 20:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:85 on 2024-10-30 20:18_

Oh.. thanks 🤦‍♀️ this got copy-pasted after several iterations or I probably would have started there.

---

_Merged by @zanieb on 2024-10-30 21:28_

---

_Closed by @zanieb on 2024-10-30 21:28_

---

_Branch deleted on 2024-10-30 21:28_

---
