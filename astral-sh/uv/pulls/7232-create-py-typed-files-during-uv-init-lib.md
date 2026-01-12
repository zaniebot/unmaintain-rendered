```yaml
number: 7232
title: "Create `py.typed` files during `uv init --lib`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/init-typed
created_at: 2024-09-09T20:45:51Z
updated_at: 2024-09-12T19:01:40Z
url: https://github.com/astral-sh/uv/pull/7232
synced_at: 2026-01-12T16:07:45Z
```

# Create `py.typed` files during `uv init --lib`

---

_@zanieb_

_No description provided._

---

_Label `enhancement` added by @zanieb on 2024-09-09 20:45_

---

_Marked ready for review by @zanieb on 2024-09-09 20:59_

---

_@charliermarsh reviewed on 2024-09-09 21:13_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:555 on 2024-09-09 21:13_

Maybe this should be within the `if !init_py.try_exists()?`... Otherwise, we're creating `src/py.typed` even if the source is contained elsewhere in an existing project.

---

_@zanieb reviewed on 2024-09-10 02:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:555 on 2024-09-10 02:19_

The code isn't super clear because of that variable name, but it's actually `src/<module>/py.typed` and `src/<module>/__init__.py`. I think this is reasonable to create even if the `__init__.py` already exists since we're (in theory) initializing the metadata to publish the library.

---

_Merged by @zanieb on 2024-09-10 20:16_

---

_Closed by @zanieb on 2024-09-10 20:16_

---

_Branch deleted on 2024-09-10 20:16_

---

_@kcarnold reviewed on 2024-09-12 18:54_

---

_Review comment by @kcarnold on `crates/uv/src/commands/project/init.rs`:555 on 2024-09-12 18:54_

Any possibility that this might clobber a file that might already exist?

---

_@zanieb reviewed on 2024-09-12 19:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:555 on 2024-09-12 19:01_

Yep 🤦‍♀️ https://github.com/astral-sh/uv/pull/7338

---
