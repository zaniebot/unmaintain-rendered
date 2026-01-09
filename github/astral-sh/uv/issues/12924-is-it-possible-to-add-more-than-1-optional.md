---
number: 12924
title: Is it possible to add more than 1 optional dependency group when using dynamic python versions?
type: issue
state: closed
author: dan-b-at-osh
labels:
  - bug
assignees: []
created_at: 2025-04-16T18:48:33Z
updated_at: 2025-04-17T13:33:51Z
url: https://github.com/astral-sh/uv/issues/12924
synced_at: 2026-01-07T13:12:18-06:00
---

# Is it possible to add more than 1 optional dependency group when using dynamic python versions?

---

_Issue opened by @dan-b-at-osh on 2025-04-16 18:48_

### Question

I can't add a second batch of `--optional` dependencies when using a dynamic version while building a workspace package. Is this expected error? Toy example below based on [this awesome repo](https://github.com/konstin/uv-workspace-example-cable/tree/main) and this modified `pyproject.toml` where I'm using `hatch-vcs` to set the version.

### Ubuntu commands
✅ Successfully added `network` `--extras` of `requests`.
❌ Unsuccessfully attempted to add `moo` `--extras` of `cowsay`.
```sh
$ ~/uv-workspace-example-cable$ git tag 0.1.0
$ ~/uv-workspace-example-cable$ uv add --optional network httpx
Resolved 12 packages in 950ms
      Built cable @ file:///home/azureuser/uv-workspace-example-cable
Prepared 8 packages in 552ms
Uninstalled 1 package in 0.77ms
Installed 8 packages in 1ms
 + anyio==4.9.0
 - cable==0.1.0 (from file:///home/azureuser/uv-workspace-example-cable)
 + cable==0.1.1.dev0+g2fbead2.d20250416 (from file:///home/azureuser/uv-workspace-example-cable)
 + certifi==2025.1.31
 + h11==0.14.0
 + httpcore==1.0.8
 + httpx==0.28.1
 + idna==3.10
 + sniffio==1.3.1
$ ~/uv-workspace-example-cable$ uv add --optional moo cowsay
Resolved 12 packages in 16ms
error: Extra `moo` is not defined in the project's `optional-dependencies` table
```

### `pyproject.toml`
```toml
[project]
name = "cable"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "konstin", email = "konstin@mailbox.org" }
]
requires-python = ">=3.12"
dependencies = []
dynamic = ["version"]

[project.optional-dependencies]
cli = [
    "cable-cli",
]
experiments = [
    "cable-experiments",
]
network = [
    "httpx>=0.28.1",
]

[tool.uv.sources]
cable-core = { workspace = true }
cable-cli = { workspace = true }
cable-experiments = { workspace = true }

[tool.uv.workspace]
members = ["packages/*"]

[build-system]
requires = ["hatchling", "hatch-vcs"]
build-backend = "hatchling.build"


[tool.hatch.version]
source = "vcs"

[tool.uv]
cache-keys = [{ git = true }]
```

You can replace the project's `pyproject.toml` with this one before running the provided commands to see the result. More than happy to provide any additional info you need - I've loved working with `uv`! Got my whole team to start using it. Can't recommend it enough 😁 

### Platform

Ubuntu 20.04 Linux 5.15.0-1073-azure x86_64 GNU/Linux

### Version

uv 0.6.14

---

_Label `question` added by @dan-b-at-osh on 2025-04-16 18:48_

---

_Comment by @zanieb on 2025-04-16 18:49_

This _seems_ like a bug? This only occurs with dynamic `version` fields?

---

_Comment by @dan-b-at-osh on 2025-04-16 19:07_

As far as I can tell, yes. When I reverted to manually setting my `version`, it behaved as expected immediately. I didn't even remove `hatch-vcs` or these fields
```toml
...
[tool.hatch.version]
source = "vcs"

[tool.uv]
cache-keys = [{ git = true }]
...
```

---

_Label `question` removed by @zanieb on 2025-04-16 19:09_

---

_Label `bug` added by @zanieb on 2025-04-16 19:09_

---

_Comment by @charliermarsh on 2025-04-17 13:26_

Oh, I think this is because of:

```toml
[tool.uv]
cache-keys = [{ git = true }]
```

Setting `cache-keys` like that will replace the default values. You want:

```toml
[tool.uv]
cache-keys = [{ file = "pyproject.toml" }, { git = true }]
```


---

_Comment by @dan-b-at-osh on 2025-04-17 13:33_

This immediately worked, thank you! Looks like I missed that part of the settings documentation. 

---

_Closed by @dan-b-at-osh on 2025-04-17 13:33_

---
