```yaml
number: 6142
title: Possible bug with Workspace.lock_path initialization
type: issue
state: closed
author: idlsoft
labels: []
assignees: []
created_at: 2024-08-16T05:38:54Z
updated_at: 2024-08-16T17:24:28Z
url: https://github.com/astral-sh/uv/issues/6142
synced_at: 2026-01-12T15:59:01Z
```

# Possible bug with Workspace.lock_path initialization

---

_@idlsoft_

A [recent change](https://github.com/astral-sh/uv/commit/e3f345ce09e47157af4d83aed76fb9275fbcf757#diff-1727b6c242607c58fed577d975fa5f8c762386d44a037188e605ac3e9a79cc7f) added this [line](https://github.com/astral-sh/uv/blob/15dfb660ab7f1ccca7e57c985968a18a85bb1d86/crates/uv-resolver/src/lock.rs#L2800) to `uv-resolver/src/lock.rs`:
```rust
let lock_path = relative_to(workspace.lock_path(), &lock_path).unwrap();
```
This line panicks if `workspace.lock_path` is initialized from `Path::new("").to_path_buf()`, which does happen [here](https://github.com/astral-sh/uv/blob/15dfb660ab7f1ccca7e57c985968a18a85bb1d86/crates/uv-workspace/src/workspace.rs#L806)
```rust
        Self::from_project(
            project_root,
            Path::new(""),
            &project,
            &pyproject_toml,
            options,
        )
```
 and [here](https://github.com/astral-sh/uv/blob/15dfb660ab7f1ccca7e57c985968a18a85bb1d86/crates/uv-workspace/src/workspace.rs#L1259).
```rust
            let project = ProjectWorkspace::from_project(
                project_root,
                Path::new(""),
                project,
                &pyproject_toml,
                options,
            )
```

I'm not sure if this ever actually triggers in the existing codebase, but it seems inconsistent, probably worth a look?

---

_Comment by @charliermarsh on 2024-08-16 11:22_

I can double-check, thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-16 11:22_

---

_Comment by @idlsoft on 2024-08-16 15:03_

FWIW replacing `Path::new("")` with `project_root` takes care of it and doesn't break any existing unit tests

---

_Closed by @charliermarsh on 2024-08-16 17:24_

---

_Closed by @charliermarsh on 2024-08-16 17:24_

---
