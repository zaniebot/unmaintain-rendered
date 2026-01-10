---
number: 15342
title: Configure index be based on host or platform
type: issue
state: closed
author: jpkolsen
labels:
  - question
assignees: []
created_at: 2025-08-18T06:52:56Z
updated_at: 2025-08-19T14:30:45Z
url: https://github.com/astral-sh/uv/issues/15342
synced_at: 2026-01-10T01:25:55Z
---

# Configure index be based on host or platform

---

_Issue opened by @jpkolsen on 2025-08-18 06:52_

### Question

I'm in a setting where our production environment does not have access to the internet so it uses a private index. I can target this with `uv` by adding an environment variable, cli flag or in pyproject.toml. Hovewer, if I want to develop locally, I don't have access to the private index. Incidentally,  my own machine is a mac while the production host is linux, so I tried adding the below to pyproject.toml (using `art` package as example)
```
dependencies = [
    "art>=6.5",
]

[tool.uv.sources]
art = [
  { index = "pypi", marker = "platform_system == 'Darwin'"},
  { index = "nexus", marker = "platform_system == 'Linux'"},
]

[[tool.uv.index]]
name = "nexus"
url = "https://internal.url/repository/pypi-proxy/simple"

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.python.org/simple"
```

This fails on both platforms. It looks like it's trying to access both indexes even though the marker specifies to use one, but maybe I'm just holding it wrong? Also, could this be set up in a way that's not specific to each package, but basically just configures `uv` to use one index on one host/platform and another index on all others?

### Platform

MacOS 15, Ubuntu 22.04

### Version

uv 0.8.11

---

_Label `question` added by @jpkolsen on 2025-08-18 06:52_

---

_Comment by @charliermarsh on 2025-08-19 09:30_

Two things:

1. I recommend adding `explicit = true` to each of your `[[tool.uv.index]]` entries. That says: "Only use this for packages that are marked with the index". Otherwise, we'll try to query it for other dependencies, which could lead to errors in your case.
2. uv creates a universal `uv.lock`, so it needs access to both indexes in order to generate the lockfile, even if it won't need to install the package from the Linux index when you're on the macOS machine. Typically what you'd do is generate the `uv.lock` on your machine that _does_ have access to the private index, then on macOS, run with the `--frozen` flag to use the lockfile but avoid attempting to update or validate it.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-19 09:30_

---

_Comment by @jpkolsen on 2025-08-19 14:11_

Thanks for confirming that locking requires access to both indexes, at least I know that I shouldn't try to fiddle with settings to get my intended workflow running! I think your workaround might work, so thanks for that suggestion as well!

---

_Closed by @jpkolsen on 2025-08-19 14:11_

---

_Comment by @charliermarsh on 2025-08-19 14:30_

No worries! Thanks for following up.

---
