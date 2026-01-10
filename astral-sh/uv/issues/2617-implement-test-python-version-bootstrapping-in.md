```yaml
number: 2617
title: Implement test Python version bootstrapping in Rust
type: issue
state: closed
author: zanieb
labels:
  - internal
assignees: []
created_at: 2024-03-22T16:49:08Z
updated_at: 2024-04-12T19:55:59Z
url: https://github.com/astral-sh/uv/issues/2617
synced_at: 2026-01-10T05:40:32Z
```

# Implement test Python version bootstrapping in Rust

---

_Issue opened by @zanieb on 2024-03-22 16:49_

Rough plan:

- Add a new crate `uv-toolchain`
- Move Python version metadata script to `uv-toolchain`?
- Add a build script which runs when `python-versions.json` changes
- Convert JSON to static Rust declaration of available versions on build
- Add method to "install" a specific toolchain version
- Add development command to install Python versions noted in the bootstrap directory
- Update Python interpreter discovery to respect bootstrap directory
- Investigate Windows support

Resources:

- https://github.com/astral-sh/rye/blob/main/rye-devtools/src/rye_devtools/find_downloads.py
- https://github.com/astral-sh/rye/blob/main/rye/src/cli/toolchain.rs
- https://github.com/astral-sh/rye/blob/153213c6af75651c63d97ae1082471c43dfa80e1/rye/src/platform.rs#L152-L173

---

_Assigned to @zanieb by @zanieb on 2024-03-22 16:49_

---

_Label `internal` added by @zanieb on 2024-03-22 16:49_

---

_Closed by @zanieb on 2024-04-12 19:55_

---
