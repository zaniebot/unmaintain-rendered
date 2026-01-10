```yaml
number: 4641
title: "Add `uv tool uninstall`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/tool-uninstall
created_at: 2024-06-29T03:48:36Z
updated_at: 2024-06-29T17:56:22Z
url: https://github.com/astral-sh/uv/pull/4641
synced_at: 2026-01-10T13:48:28Z
```

# Add `uv tool uninstall`

---

_Pull request opened by @zanieb on 2024-06-29 03:48_

_No description provided._

---

_Label `cli` added by @zanieb on 2024-06-29 03:48_

---

_Label `preview` added by @zanieb on 2024-06-29 03:48_

---

_Marked ready for review by @zanieb on 2024-06-29 04:31_

---

_@charliermarsh approved on 2024-06-29 16:49_

---

_@charliermarsh reviewed on 2024-06-29 16:50_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/uninstall.rs`:37 on 2024-06-29 16:50_

I would suggest that this handles non-existence gracefully? Otherwise, there's no way to uninstall a tool that has been partially uninstalled (for whatever reason).

---

_@zanieb reviewed on 2024-06-29 17:04_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:37 on 2024-06-29 17:04_

Yeah sounds good to me!

---

_Comment by @charliermarsh on 2024-06-29 17:11_

Want me to edit + merge and include in the release?

---

_Comment by @zanieb on 2024-06-29 17:18_

Feel free. I won't be around for most of today.

---

_Comment by @charliermarsh on 2024-06-29 17:29_

I think we also need to remove the receipt, otherwise tools continue to appear in `uv tool list`:

```
puffin on  zb/tool-uninstall [$] is 📦 v0.2.17 via 🐍 v3.12.3 via 🦀 v1.78.0 took 3s
❯ cargo run tool uninstall black
   Compiling uv-cli v0.0.1 (/Users/crmarsh/workspace/puffin/crates/uv-cli)
   Compiling uv v0.2.17 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.45s
     Running `target/debug/uv tool uninstall black`
warning: `uv tool uninstall` is experimental and may change without warning.
Uninstalled: black, blackd
puffin on  zb/tool-uninstall [$] is 📦 v0.2.17 via 🐍 v3.12.3 via 🦀 v1.78.0 took 3s
❯ cargo run tool list
   Compiling uv-cli v0.0.1 (/Users/crmarsh/workspace/puffin/crates/uv-cli)
   Compiling uv v0.2.17 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.46s
     Running `target/debug/uv tool list`
warning: `uv tool list` is experimental and may change without warning.
black
```

---

_Merged by @charliermarsh on 2024-06-29 17:50_

---

_Closed by @charliermarsh on 2024-06-29 17:50_

---

_Branch deleted on 2024-06-29 17:50_

---

_Comment by @zanieb on 2024-06-29 17:55_

Oh sorry that was kind of an obvious oversight. Thanks!

---

_Comment by @charliermarsh on 2024-06-29 17:56_

Np I added some coverage too.

---
