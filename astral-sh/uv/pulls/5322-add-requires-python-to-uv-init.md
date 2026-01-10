```yaml
number: 5322
title: "Add `requires-python` to `uv init`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/init-requires-python
created_at: 2024-07-23T01:03:59Z
updated_at: 2024-07-23T16:02:41Z
url: https://github.com/astral-sh/uv/pull/5322
synced_at: 2026-01-10T13:37:23Z
```

# Add `requires-python` to `uv init`

---

_Pull request opened by @charliermarsh on 2024-07-23 01:03_

## Summary

Prefers, in order:

- The major-minor version of an interpreter discovered via `--python`.
- The `requires-python` from the workspace.
- The major-minor version of the default interpreter.

If the `--python` request is a version or a version range, we use that without fetching an interpreter.

Closes https://github.com/astral-sh/uv/issues/5299.


---

_Label `bug` added by @charliermarsh on 2024-07-23 01:09_

---

_Label `preview` added by @charliermarsh on 2024-07-23 01:09_

---

_@charliermarsh reviewed on 2024-07-23 01:10_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:137 on 2024-07-23 01:10_

@zanieb - Can you review that what I'm doing with the discovery here is reasonable?

---

_Review requested from @zanieb by @charliermarsh on 2024-07-23 02:50_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-07-23 02:50_

---

_@konstin approved on 2024-07-23 08:53_

---

_Comment by @FishAlchemist on 2024-07-23 13:32_

![image](https://github.com/user-attachments/assets/e68e3947-ff51-4ed5-a39a-b67b9c8fbee6)
Should we avoid creating pyproject.toml when providing ``python version`` causes an error? 
This is because creating it would lead to ``Project is already initialized``. 
![image](https://github.com/user-attachments/assets/b5d2c0cf-ecc6-446c-aa4a-027424e38869)
A failed creation will result in ``pyproject.toml`` being created, but missing ``requires-python``
e.g.
```toml
[project]
name = "www"
version = "0.1.0"

```


---

_Comment by @charliermarsh on 2024-07-23 13:43_

Yeah... Though unfortunately we need to create the `pyproject.toml` to discover the workspace :(

I'll figure something out.

---

_Comment by @j178 on 2024-07-23 14:05_

Just a heads-up,`cargo init` checks `workspace.members` glob pattern before creating the project:

https://github.com/rust-lang/cargo/blob/7f2079838ac6e0a3cb05b1b9dac4401a76572855/src/cargo/ops/cargo_new.rs#L978-L996

---

_Comment by @charliermarsh on 2024-07-23 14:52_

@j178 - We also check that before modifying the workspace manifest.

---

_@ibraheemdev approved on 2024-07-23 15:02_

---

_Merged by @charliermarsh on 2024-07-23 16:02_

---

_Closed by @charliermarsh on 2024-07-23 16:02_

---

_Branch deleted on 2024-07-23 16:02_

---
