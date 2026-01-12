```yaml
number: 9292
title: "`dynamic = [\"version\"]` causes issues with `[tool.uv.sources]`"
type: issue
state: closed
author: aglove2189
labels:
  - bug
assignees: []
created_at: 2024-11-20T21:19:37Z
updated_at: 2024-11-20T21:27:36Z
url: https://github.com/astral-sh/uv/issues/9292
synced_at: 2026-01-12T15:59:46Z
```

# `dynamic = ["version"]` causes issues with `[tool.uv.sources]`

---

_@aglove2189_

Running on Debian with uv 0.5.3. Using `dynamic = ["version"]` in conjunction with `[tool.uv.sources]` causes this error:

```sh
> uv lock
  × Failed to build `project @ file:///home/repos/uv-test`
  ╰─▶ Source entry for `torch` only applies to extra `cpu`, but the `cpu` extra does not exist. When an extra is present
      on a source (e.g., `extra = "cpu"`), the relevant package must be included in the `project.optional-dependencies`
      section for that extra (e.g., `project.optional-dependencies = { "cpu" = ["torch"] }`).
```

Switching to a static version, e.g. `version = "0.0.1"` will allow `uv` to lock successfully.

```toml
[project]
name = "project"
dynamic = ["version"]
requires-python = ">=3.12.0"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
]
cu124 = [
  "torch>=2.5.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

---

_Label `bug` added by @charliermarsh on 2024-11-20 21:20_

---

_Comment by @charliermarsh on 2024-11-20 21:20_

I think this is fixed in the release that's going out now.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 21:20_

---

_Comment by @aglove2189 on 2024-11-20 21:23_

@charliermarsh you are a machine, response time is unreal. I will test with next release, thank you!

---

_Comment by @charliermarsh on 2024-11-20 21:23_

Can you try with [uv 0.5.4](https://pypi.org/project/uv/0.5.4/)?

---

_Comment by @aglove2189 on 2024-11-20 21:24_

works with 0.5.4!

---

_Closed by @aglove2189 on 2024-11-20 21:24_

---

_Comment by @charliermarsh on 2024-11-20 21:27_

Amazing, thanks!

---
