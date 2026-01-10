```yaml
number: 5160
title: "Add `uv tool dir --bin` to show executable directory"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/bin
created_at: 2024-07-17T18:13:45Z
updated_at: 2024-07-18T14:30:25Z
url: https://github.com/astral-sh/uv/pull/5160
synced_at: 2026-01-10T13:42:52Z
```

# Add `uv tool dir --bin` to show executable directory

---

_Pull request opened by @charliermarsh on 2024-07-17 18:13_

## Summary

Closes https://github.com/astral-sh/uv/issues/5159.


---

_Marked ready for review by @charliermarsh on 2024-07-17 18:13_

---

_Label `cli` added by @charliermarsh on 2024-07-17 18:13_

---

_Label `preview` added by @charliermarsh on 2024-07-17 18:13_

---

_Merged by @charliermarsh on 2024-07-17 20:30_

---

_Closed by @charliermarsh on 2024-07-17 20:30_

---

_Branch deleted on 2024-07-17 20:30_

---

_@zanieb reviewed on 2024-07-18 03:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/dir.rs`:20 on 2024-07-18 03:40_

Fyi I think this is infallible. The error case comes from `StateStore::from_path` which uses a `Result` but returns an `Err`?

---

_@zanieb reviewed on 2024-07-18 03:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/dir.rs`:17 on 2024-07-18 03:41_

Is there a reason you opted for the full name here? I think it's standard to do

```
use anstream::{eprintln, println};
```

nearly everywhere else.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2178 on 2024-07-18 03:42_

Was there a particular motivation for adding an alias?

---

_@zanieb reviewed on 2024-07-18 03:42_

---

_@charliermarsh reviewed on 2024-07-18 13:58_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2178 on 2024-07-18 13:58_

In the moment it felt odd that we use the term "executable" everywhere but `--bin` for the directory. I'll remove it though.

---

_@charliermarsh reviewed on 2024-07-18 13:58_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/dir.rs`:17 on 2024-07-18 13:58_

Consistency with the rest of the file.

---

_@charliermarsh reviewed on 2024-07-18 13:59_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/dir.rs`:20 on 2024-07-18 13:59_

Do you know why that tree of functions returns `Result` if it never returns `Err`?

---

_@charliermarsh reviewed on 2024-07-18 14:04_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2178 on 2024-07-18 14:04_

Fixed: https://github.com/astral-sh/uv/pull/5187

---

_@charliermarsh reviewed on 2024-07-18 14:04_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/dir.rs`:17 on 2024-07-18 14:04_

Fixed: https://github.com/astral-sh/uv/pull/5187

---

_@zanieb reviewed on 2024-07-18 14:30_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/dir.rs`:20 on 2024-07-18 14:30_

It was copied from the `Cache` implementation, but someone dropped the `Result` from there in https://github.com/astral-sh/uv/pull/3875

The tree should be updated. I can do it if you want.

---
