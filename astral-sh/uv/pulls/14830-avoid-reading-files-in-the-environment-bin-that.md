```yaml
number: 14830
title: Avoid reading files in the environment bin that are not entrypoints
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-entry
created_at: 2025-07-22T18:41:33Z
updated_at: 2025-07-22T19:11:16Z
url: https://github.com/astral-sh/uv/pull/14830
synced_at: 2026-01-12T16:11:26Z
```

# Avoid reading files in the environment bin that are not entrypoints

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/14829

I tested this against the given Dockerfile.

---

_Label `bug` added by @zanieb on 2025-07-22 18:41_

---

_Renamed from "Avoid reading files in the environment bin that are not entrypointse" to "Avoid reading files in the environment bin that are not entrypoints" by @zanieb on 2025-07-22 18:41_

---

_@zanieb reviewed on 2025-07-22 18:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1784 on 2025-07-22 18:43_

We could use a bigger buffer here to scan for our expected shebang. It gets a little more complicated though, because then we need to make assumptions about the length of the file path? I guess we could cast `previous_executable` to a byte length and use a constant for the reloctable header?

In some world, it'd be ideal to stream reading from the source to the other file too, instead of reading it all to memory.

---

_@charliermarsh reviewed on 2025-07-22 18:44_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1784 on 2025-07-22 18:44_

It seems okay as it is, but I'm wondering if we should make non-UTF-8 non-fatal? Is it possible that a binary file could happen to start with the byte equivalents of `#!`?

---

_@zanieb reviewed on 2025-07-22 18:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1784 on 2025-07-22 18:46_

I was considering that too... it can't hurt to make it non-fatal, it seems pretty unlikely in either case.

---

_Marked ready for review by @zanieb on 2025-07-22 18:48_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 18:54_

Do you want to match on a more specific variant?

---

_@charliermarsh approved on 2025-07-22 18:54_

---

_@zanieb reviewed on 2025-07-22 19:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1789 on 2025-07-22 19:01_

From reading the documentation I assumed that was the only error variant, which... is silly.

---

_Merged by @zanieb on 2025-07-22 19:11_

---

_Closed by @zanieb on 2025-07-22 19:11_

---

_Branch deleted on 2025-07-22 19:11_

---
