```yaml
number: 4248
title: Improve output when an older toolchain version is already installed
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
  - preview
assignees: []
merged: true
base: main
head: zb/install
created_at: 2024-06-11T19:30:49Z
updated_at: 2024-06-11T19:49:16Z
url: https://github.com/astral-sh/uv/pull/4248
synced_at: 2026-01-10T13:54:02Z
```

# Improve output when an older toolchain version is already installed

---

_Pull request opened by @zanieb on 2024-06-11 19:30_

e.g.

```
❯ uv toolchain install
Found installed toolchain 'cpython-3.9.19-macos-aarch64-none'
A toolchain is already installed. Use `uv toolchain install <request>` to install a specific toolchain
```

instead of

```
❯ uv toolchain install
Using latest Python version
Found installed toolchain 'cpython-3.9.19-macos-aarch64-none'
Already installed at /Users/zb/Library/Application Support/uv/toolchains/cpython-3.9.19-macos-aarch64-none
```

---

_Label `tracing` added by @zanieb on 2024-06-11 19:30_

---

_Label `preview` added by @zanieb on 2024-06-11 19:30_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/toolchain/install.rs`:64 on 2024-06-11 19:39_

Nit: end in period for consistency?

---

_@charliermarsh approved on 2024-06-11 19:39_

---

_@zanieb reviewed on 2024-06-11 19:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/install.rs`:64 on 2024-06-11 19:40_

👍 

---

_Merged by @zanieb on 2024-06-11 19:49_

---

_Closed by @zanieb on 2024-06-11 19:49_

---

_Branch deleted on 2024-06-11 19:49_

---
