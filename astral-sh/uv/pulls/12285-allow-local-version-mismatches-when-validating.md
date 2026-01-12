```yaml
number: 12285
title: Allow local version mismatches when validating lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/local
created_at: 2025-03-18T14:58:24Z
updated_at: 2025-03-18T15:12:42Z
url: https://github.com/astral-sh/uv/pull/12285
synced_at: 2026-01-12T16:10:12Z
```

# Allow local version mismatches when validating lockfile

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/12282.

## Test Plan

Given:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = ["flash-attn"]

[tool.uv]
environments = ["sys_platform == 'darwin'", "sys_platform == 'linux'"]
constraint-dependencies = ["torch==2.5.1"]

[tool.uv.sources]
flash_attn = [
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp310-cp310-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.10'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp311-cp311-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.11'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp312-cp312-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.12'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp313-cp313-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.13'"}
]
```

Ran `uv lock` on `v0.6.5`. Then verified that `uv lock` fails on `v0.6.6` on the same lockfile, but this commit succeeds.


---

_Label `bug` added by @charliermarsh on 2025-03-18 14:58_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:2877 on 2025-03-18 14:59_

nit: `if version.without_local() != &wheel.filename.version.without_local()` to be fully permissive?

---

_@konstin approved on 2025-03-18 15:00_

---

_@charliermarsh reviewed on 2025-03-18 15:00_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:2877 on 2025-03-18 15:00_

I don't _think_ we want to allow mismatching locals -- we don't do that elsewhere, at least? I think it would error elsewhere, like when we install.

---

_Marked ready for review by @charliermarsh on 2025-03-18 15:04_

---

_Merged by @charliermarsh on 2025-03-18 15:12_

---

_Closed by @charliermarsh on 2025-03-18 15:12_

---

_Branch deleted on 2025-03-18 15:12_

---
