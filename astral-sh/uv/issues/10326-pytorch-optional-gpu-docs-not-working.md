```yaml
number: 10326
title: PyTorch optional GPU docs not working
type: issue
state: closed
author: jbohnslav
labels:
  - needs-mre
assignees: []
created_at: 2025-01-06T13:33:54Z
updated_at: 2025-01-22T21:32:13Z
url: https://github.com/astral-sh/uv/issues/10326
synced_at: 2026-01-12T16:00:11Z
```

# PyTorch optional GPU docs not working

---

_@jbohnslav_

I found the [Configuring accelerators with optional dependencies](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies) section of the PyTorch docs didn't work in my case. 

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.10.0"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
]
cu121 = [
  "torch>=2.5.1",
]

[tool.uv]
conflicts = [
  [
    {extra = "cpu"},
    {extra = "cu121"},
  ],
]

[tool.uv.sources]
torch = [
  {index = "pytorch-cpu", extra = "cpu"},
  {index = "pytorch-cu121", extra = "cu121"},
]

[[tool.uv.index]]
explicit = true
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"

[[tool.uv.index]]
explicit = true
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
```

Executing
```bash
uv sync --extra cpu
```

Gave this error:
```bash
error: Distribution `torch==2.5.1 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

I had to explicitly set the version in the optional-dependencies section:
```toml
[project.optional-dependencies]
cpu = [
  "torch==2.5.1+cpu",
]
cu121 = [
  "torch==2.5.1+cu121",
]
```


---

_Comment by @charliermarsh on 2025-01-06 14:31_

It works fine for me. I'd need more context. What uv version are you on? What machine are you on?

---

_Label `needs-mre` added by @charliermarsh on 2025-01-06 14:31_

---

_Comment by @jbohnslav on 2025-01-06 18:08_

uv version: `0.5.14`. 
Machine: `Ubuntu 22.04, x86`

---

_Comment by @charliermarsh on 2025-01-06 18:11_

Are you trying to use Python 3.13 or something? Can you share the generated lockfile?

---

_Comment by @sglbl on 2025-01-07 13:05_

@jbohnslav I'm not an uv maintainer. But whenever I got a problem about uv, I remove the uv.lock and I try again. Then it works. I had a similar error and removing uv.lock fixed the issue for me. I suggest you to try that

---

_Comment by @zanieb on 2025-01-07 18:24_

>  I suggest you to try that and add uv.lock to .gitignore

I would not recommend this — the lockfile is important to check into version control.

---

_Comment by @charliermarsh on 2025-01-22 21:26_

Closing for now due to a lack of reproducible instructions.

---

_Closed by @charliermarsh on 2025-01-22 21:26_

---
