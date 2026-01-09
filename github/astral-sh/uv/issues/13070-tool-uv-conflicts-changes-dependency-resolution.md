---
number: 13070
title: "`tool.uv.conflicts` changes dependency resolution even without conflicts"
type: issue
state: open
author: rambip
labels:
  - bug
assignees: []
created_at: 2025-04-23T15:22:30Z
updated_at: 2025-04-25T02:06:07Z
url: https://github.com/astral-sh/uv/issues/13070
synced_at: 2026-01-07T13:12:18-06:00
---

# `tool.uv.conflicts` changes dependency resolution even without conflicts

---

_Issue opened by @rambip on 2025-04-23 15:22_

# Problem

In some situations, adding a dummy `tool.uv.conflicts` changes the package resolution with specified sources.

It is a completely undocumented behaviour, and in the specific case below, the only way to implement what I want is very hacky.

# Reproduce:

First, create a pyproject.toml with this content:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "bar"
dependencies = ["torch==2.6.0"]
requires-python = ">=3.10"

[project.optional-dependencies]
cpu = ["torch==2.6.0"]

[tool.uv.sources]
torch = [{ index = "torch-cpu", extra = "cpu" }]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv]
conflicts = [[{ extra = "cpu" }, { extra = "this_extra_does_not_even_exist" }]]
```

This works as expected: if you do `uv sync`, it install the gpu version of pytorch. If you try `uv sync --extra cpu`, it installs the cpu version from pytorch.org.

**Note 1**: you don't need the name of the extra to exist !

**Note 2**: In a real world scenario, `torch` would be deeper in the dependency tree. I did not find any other workaround to specify "install the cpu version only when this extra is enabled". 

Now, just comment out the last line of the config:
```toml
[project]
name = "foo"
version = "0.1.0"
description = "bar"
dependencies = ["torch==2.6.0"]
requires-python = ">=3.10"

[project.optional-dependencies]
cpu = ["torch==2.6.0"]

[tool.uv.sources]
torch = [{ index = "torch-cpu", extra = "cpu" }]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv]
# conflicts = [[{ extra = "cpu" }, { extra = "this_extra_does_not_even_exist" }]]
```

Now, `uv sync` will download the cpu version even though the index is specified for 

# Expected behaviour

They are 2 misleading things in this situation:
- first, `uv` should complain that the extra `this_extra_does_not_even_exist` is invalid
- more importantly, `uv` sould not use the specified index if the extra is not enabled.

It may be an issue with the python PEP, but I have to say it is very confusing.


### Platform

Linux (fedora)

### Version

0.6.15

### Python version

3.10

---

_Label `bug` added by @rambip on 2025-04-23 15:22_

---

_Renamed from "`tool.uv.conflicts` change dependency resolution even without conflicts" to "`tool.uv.conflicts` changes dependency resolution even without conflicts" by @rambip on 2025-04-23 15:25_

---

_Referenced in [astral-sh/uv#13073](../../astral-sh/uv/issues/13073.md) on 2025-04-23 19:31_

---

_Comment by @charliermarsh on 2025-04-25 01:38_

Yeah it does seem like a bug that the Torch index is being used in the second snippet -- I'd expect it to fall back to PyPI.

---

_Comment by @charliermarsh on 2025-04-25 02:06_

I think we need the presence of the `extra` marker on the source to initiate a fork.

---
