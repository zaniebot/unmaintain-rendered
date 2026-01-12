```yaml
number: 4312
title: "Add `EXTERNALLY-MANAGED` markers to managed toolchains"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/externally-managed
created_at: 2024-06-13T18:00:35Z
updated_at: 2024-06-17T15:25:35Z
url: https://github.com/astral-sh/uv/pull/4312
synced_at: 2026-01-12T16:06:09Z
```

# Add `EXTERNALLY-MANAGED` markers to managed toolchains

---

_@zanieb_

Closes #4240 

e.g.

```
❯ cargo run -q -- pip install anyio --python "/Users/zb/Library/Application Support/uv/toolchains/cpython-3.12.0-macos-aarch64-none/install/bin/python3"
error: The interpreter at /Users/zb/Library/Application Support/uv/toolchains/cpython-3.12.0-macos-aarch64-none/install is externally managed, and indicates the following:

  This toolchain is managed by uv and should not be modified.

Consider creating a virtual environment with `uv venv`.
```

---

_Label `preview` added by @zanieb on 2024-06-13 18:00_

---

_Marked ready for review by @zanieb on 2024-06-13 18:11_

---

_@charliermarsh reviewed on 2024-06-13 22:59_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/managed.rs`:200 on 2024-06-13 22:59_

Should we suggest using a virtual environment? (Have you looked at any of the other externally-managed messages?)

---

_@charliermarsh approved on 2024-06-13 23:45_

---

_@charliermarsh reviewed on 2024-06-13 23:45_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/managed.rs`:299 on 2024-06-13 23:45_

Is this correct on all platforms?

---

_@zanieb reviewed on 2024-06-14 00:28_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:200 on 2024-06-14 00:28_

I did at first but we suggest using a virtual environment in our own error message it seemed overkill. I guess they could be using `pip` but we don't expose a central `bin` for our managed toolchains so I don't expect them to be on the PATH.

There's a Debian example over at https://packaging.python.org/en/latest/specifications/externally-managed-environments/#marking-an-interpreter-as-using-an-external-package-manager 

I figure we could expand on this with more concrete recommendations if we need to later?

---

_@zanieb reviewed on 2024-06-14 00:29_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:299 on 2024-06-14 00:29_

Good question.. umm... I guess I'll need to extract a Windows one and check!

---

_@charliermarsh reviewed on 2024-06-14 00:34_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/managed.rs`:299 on 2024-06-14 00:34_

I think technically you need to query the interpreter for this.

---

_@zanieb reviewed on 2024-06-14 00:40_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:299 on 2024-06-14 00:40_

Technically, yep! That's how I found out where to put it. But

- This struct doesn't have access to an interpreter right now
- It seems expensive to query Python to check for this file
- This is just for the managed toolchains where I think we can assume a consistent structure

---

_@charliermarsh reviewed on 2024-06-14 04:18_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/managed.rs`:299 on 2024-06-14 04:18_

That makes sense. If it's just for the managed Pythons, we may be able to make some assumptions. (Won't we need to query Python eventually any way though?)

---

_@zanieb reviewed on 2024-06-14 14:32_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:299 on 2024-06-14 14:32_

We do but not until you "use" the toolchain — in `uv toolchain install` we never actually need to query the interpreter. Idk how fast the operation really needs to be since it'll be dominated by the time it takes to download but it's not like we're already doing it.

---

_@zanieb reviewed on 2024-06-14 18:56_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:299 on 2024-06-14 18:56_

The Windows builds use `Lib` so this should be generally fine? I guess I'll capitalize it in that case.

---

_Merged by @zanieb on 2024-06-17 15:25_

---

_Closed by @zanieb on 2024-06-17 15:25_

---

_Branch deleted on 2024-06-17 15:25_

---
