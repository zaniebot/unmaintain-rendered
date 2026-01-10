---
number: 17187
title: Could uv overwrite the github auth acess (ssh vs https) when installing (transitive) private repos
type: issue
state: open
author: ecobost
labels:
  - enhancement
assignees: []
created_at: 2025-12-19T08:16:03Z
updated_at: 2025-12-19T09:03:02Z
url: https://github.com/astral-sh/uv/issues/17187
synced_at: 2026-01-10T01:26:14Z
---

# Could uv overwrite the github auth acess (ssh vs https) when installing (transitive) private repos

---

_Issue opened by @ecobost on 2025-12-19 08:16_

### Summary

I want to install a library, call it `data-tools` (itself a private repo but unimportant for the use case) that depends on a private repo `data-readers` as 
```toml
dependencies=[
  data-readers
]
[tool.uv.sources]
data-readers = { git = "https://github.com/private_org/data-readers.git" }
```
This forces anyone installing the package to use one form of authentication (in this case https rather than ssh). Even if my own pyproject.toml uses:
```toml
dependencies=[
  data-readers
  data-tools
]
[tool.uv.sources]
data-readers = {git = "ssh://git@github.com/private_org/data-readers.git" }
```
This will be able to install data-readers over ssh by itself but once I add the data-tools dependency it will fail asking for https authentication:
```
Failed to download and build `data-readers @ git+https://github.com/private_org/data-readers.git`
``` 
or if both https and ssh authentication are set up, it fails as:
```
    Updated ssh://git@github.com/private_org/data-readers.git
    Updated https://github.com/private_org/data-readers.git
  × Failed to resolve dependencies for `data-tools` (v0.1.4)
  ╰─▶ Requirements contain conflicting URLs for package `data-readers` in all marker environments:
      - git+https://github.com/private_org/rdata-readers.git
      - git+ssh://git@github.com/private_org/data-readers.git
```

### Example

If somebody wanted to change authentication methods then uv sources could allow to override that. Maybe even without an explicit dependency
```toml
dependencies=[
  # data-readers
  data-tools
]
[tool.uv.sources]
data-readers = {git = "ssh://git@github.com/private_org/data-readers.git" }
```
This would not change any intrinsic dependency as would picking a different tag/branch/commit so it would not need any special checks.


---

_Label `enhancement` added by @ecobost on 2025-12-19 08:16_

---

_Comment by @ecobost on 2025-12-19 08:31_

In my real use case `data_tools` is already a private repo (and the only direct dependency I have) so I was hoping that:
```toml
dependencies=[
  data-tools
]
[tool.uv.sources]
data-tools = {git = "ssh://git@github.com/private_org/data-tools.git" }
```
already told uv to use ssh authentication for any git operations related to installing data-tools (in this case fetching the data-readers repo)

---
