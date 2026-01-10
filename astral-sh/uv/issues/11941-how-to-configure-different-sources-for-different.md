---
number: 11941
title: "How to Configure Different Sources for Different `dependency-groups` Like `optional-dependencies`"
type: issue
state: closed
author: pplmx
labels:
  - question
assignees: []
created_at: 2025-03-04T05:44:03Z
updated_at: 2025-11-04T06:52:13Z
url: https://github.com/astral-sh/uv/issues/11941
synced_at: 2026-01-10T01:25:13Z
---

# How to Configure Different Sources for Different `dependency-groups` Like `optional-dependencies`

---

_Issue opened by @pplmx on 2025-03-04 05:44_

### Question

#### My Configuration: 

```toml
# ...

[dependency-groups]
cuda = [
    "torch>=2.6.0",
    "megatron-core",
]
custom = [
    "torch",
    "megatron-core",
]

[tool.uv.sources]
megatron-core = [
    { git = "https://github.com/NVIDIA/Megatron-LM", tag = "core_r0.8.0", extra = "cuda" },
    { git = "http://xxx.git", branch = "main", extra = "custom" },
]
torch = { url = "http://xxx.whl", extra = "custom" }

# ...
```

Currently, this configuration throws an error because `extra` seems to only support `optional-dependencies`. Is there a way to configure extra sources for different `dependency-groups` in a similar manner? Any suggestions or workarounds would be greatly appreciated!

#### Log

```txt
$ uv sync
Using CPython 3.10.12 interpreter at: /usr/bin/python3.10
Creating virtual environment at: .venv
error: Failed to generate package metadata for `megatron-test==0.0.1 @ editable+.`
  Caused by: Source entry for `megatron-core` only applies to extra `cuda`, but the `cuda` extra does not exist. When an extra is present on a source (e.g., `extra = "cuda"`), the relevant package must be included in the `project.optional-dependencies` section for that extra (e.g., `project.optional-dependencies = { "cuda" = ["megatron-core"] }`).
```

### Platform

Ubuntu 22.04

### Version

uv 0.6.4

---

_Label `question` added by @pplmx on 2025-03-04 05:44_

---

_Comment by @pplmx on 2025-03-04 06:22_

I've found the solution! The `dependency-groups` use `group` for identification.  

Here’s the correct configuration:  

```toml
# ...

[dependency-groups]
cuda = [
    "torch>=2.6.0",
    "megatron-core",
]
custom = [
    "torch",
    "megatron-core",
]

[tool.uv]
conflicts = [
    [
        { group = "cuda" },
        { group = "custom" },
    ],
]

[tool.uv.sources]
megatron-core = [
    { git = "https://github.com/NVIDIA/Megatron-LM", tag = "core_r0.8.0", group = "cuda" },
    { git = "http://xxx.git/", branch = "main", group = "custom" },
]
torch = { url = "http://xxx.whl/", group = "custom" }

# ...
```

---

_Closed by @pplmx on 2025-03-04 06:22_

---

_Renamed from "How to Configure Extra Sources for Different `dependency-groups` Like `optional-dependencies`" to "How to Configure Different Sources for Different `dependency-groups` Like `optional-dependencies`" by @pplmx on 2025-03-04 06:26_

---

_Comment by @pplmx on 2025-03-04 07:04_

Hi @team,  

I think it would be helpful to add more documentation on `dependency-groups` and the use of `group`. The solution I provided is based on my best guess, as I couldn't find direct documentation on configuring `uv.sources` with `group`.  

The only reference to `group` I found in the documentation is in the **conflicts** section: [Conflicting Dependencies](https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies).  

Clarifying this in the official docs would be greatly appreciated! 🚀

---

_Referenced in [astral-sh/uv#9258](../../astral-sh/uv/issues/9258.md) on 2025-04-23 17:14_

---

_Referenced in [astral-sh/uv#13073](../../astral-sh/uv/issues/13073.md) on 2025-04-23 19:27_

---

_Comment by @leo2www on 2025-11-04 06:52_

[Configuring accelerators with optional dependencies](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies)
I think this doc is good example of multi source, conficts and command running.

---
