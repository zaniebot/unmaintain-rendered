```yaml
number: 4361
title: "Respect `.python-version` files and fetch manged toolchains in uv project commands"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-file-project
created_at: 2024-06-17T16:43:39Z
updated_at: 2024-06-18T14:43:53Z
url: https://github.com/astral-sh/uv/pull/4361
synced_at: 2026-01-10T13:54:02Z
```

# Respect `.python-version` files and fetch manged toolchains in uv project commands

---

_Pull request opened by @zanieb on 2024-06-17 16:43_

As in #4360, updates the uv project CLI to respect `.python-version` files as default Python version requests. Additionally, updates project interpreter discovery to fetch managed toolchains as in `uv venv --preview`.

---

_Label `preview` added by @zanieb on 2024-06-17 16:43_

---

_Comment by @zanieb on 2024-06-17 16:47_

I'd like to add a test case here

---

_Review requested from @BurntSushi by @zanieb on 2024-06-17 16:57_

---

_Review requested from @konstin by @zanieb on 2024-06-17 16:57_

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:149 on 2024-06-17 17:03_

Nit: Maybe an if-let-else chain like `if let Some (x) = ... { x } else ...` instead of nested matching?

---

_@konstin approved on 2024-06-17 17:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:149 on 2024-06-17 17:13_

Hm interesting. I started with using the option helpers e.g. `or_else` but switched to a match because I needed to make an async call. Maybe the `if let` will be prettier.

---

_@zanieb reviewed on 2024-06-17 17:13_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/mod.rs`:149 on 2024-06-17 17:39_

I could go either way on this one personally.

---

_@BurntSushi approved on 2024-06-17 17:39_

---

_Comment by @zanieb on 2024-06-18 03:29_

I started writing test cases in a follow-up.

---

_@zanieb reviewed on 2024-06-18 14:22_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:149 on 2024-06-18 14:22_

Yeah that's actually way better. Thanks!

---

_Merged by @zanieb on 2024-06-18 14:43_

---

_Closed by @zanieb on 2024-06-18 14:43_

---

_Branch deleted on 2024-06-18 14:43_

---
