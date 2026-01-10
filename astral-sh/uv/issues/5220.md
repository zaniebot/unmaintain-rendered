```yaml
number: 5220
title: "uv lock: regression in handling local version identifiers"
type: issue
state: closed
author: ewianda
labels:
  - bug
  - lock
assignees: []
created_at: 2024-07-19T14:21:10Z
updated_at: 2024-07-19T21:56:10Z
url: https://github.com/astral-sh/uv/issues/5220
synced_at: 2026-01-10T04:53:49Z
```

# uv lock: regression in handling local version identifiers

---

_Issue opened by @ewianda on 2024-07-19 14:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
```bash
$ cat pyproject.toml 
[project]
name = "test"
version = "0.1.0"
description = "test"
dependencies = [
  "sglang[srt]==0.1.19 ;sys_platform == 'linux'",
  "torch==2.3.0+cu118 ;sys_platform == 'linux'",
]

```

```bash
$ uv --version
uv 0.2.25
$ uv lock --extra-index-url https://download.pytorch.org/whl/cu118 --index-strategy=unsafe-first-match 
warning: `uv lock` is experimental and may change without warning.
warning: No `requires-python` field found in the workspace. Defaulting to `>=3.11`.
Resolved 127 packages in 1.14s
```

```bash
$ rm uv.lock 
$ uv self update 
info: Checking for updates...
success: Upgraded `uv` to v0.2.26! https://github.com/astral-sh/uv/releases/tag/0.2.26
$ uv lock --extra-index-url https://download.pytorch.org/whl/cu118 --index-strategy=unsafe-first-match 
warning: `uv lock` is experimental and may change without warning.
warning: No `requires-python` field found in the workspace. Defaulting to `>=3.11`.
  × No solution found when resolving dependencies:
  ╰─▶ Because vllm==0.5.1 depends on torch==2.3.0 and sglang[srt]==0.1.19 depends on vllm==0.5.1, we can conclude that sglang[srt]==0.1.19 and torch{sys_platform == 'linux'}==2.3.0+cu118 are
      incompatible.
      And because test==0.1.0 depends on sglang[srt]==0.1.19 and torch{sys_platform == 'linux'}==2.3.0+cu118, we can conclude that test==0.1.0 cannot be used.
      And because only test==0.1.0 is available and you require test, we can conclude that the requirements are unsatisfiable.


---

_Label `bug` added by @zanieb on 2024-07-19 14:23_

---

_Label `lock` added by @zanieb on 2024-07-19 14:23_

---

_Comment by @charliermarsh on 2024-07-19 14:25_

Presumedly #5104. @ibraheemdev, can you take a look?

---

_Assigned to @ibraheemdev by @charliermarsh on 2024-07-19 16:08_

---

_Closed by @zanieb on 2024-07-19 21:56_

---
