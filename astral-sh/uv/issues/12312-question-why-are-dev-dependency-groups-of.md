---
number: 12312
title: "Question: why are dev dependency-groups of workspace members not being installed?"
type: issue
state: closed
author: RensDimmendaal
labels:
  - question
assignees: []
created_at: 2025-03-19T13:38:09Z
updated_at: 2025-03-19T21:46:23Z
url: https://github.com/astral-sh/uv/issues/12312
synced_at: 2026-01-10T01:25:18Z
---

# Question: why are dev dependency-groups of workspace members not being installed?

---

_Issue opened by @RensDimmendaal on 2025-03-19 13:38_

### Question

Environment
UV Version: `uv 0.6.8 (c1ef48276 2025-03-18)`
Python Version: 3.13.0 
OS: Mac OS

## Issue Description
I've set up a UV workspace with multiple packages that each have their own dev dependencies defined in the [dependency-groups] section. When I run commands that should install dev dependencies, only the root workspace's dev dependencies get installed, but the dev dependencies from the workspace members are missing.

Could someone please tell me if I am doing something wrong or if I perhaps have the wrong expectation of what can be accomplished with workspaces and/or dependency-groups?

## Steps to Reproduce

dir tree:
```
.
├── README.md
├── hello.py
├── pyproject.toml
├── uv.lock
└── ws
    └── my-ws-member
        ├── README.md
        ├── hello.py
        └── pyproject.toml
```

`./pyproject.toml`:
```
[project]
name = "test-ws"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = ["my-ws-member"]

[tool.uv.sources]
my-ws-member = { workspace = true }

[tool.uv.workspace]
members = ["ws/*"]

[dependency-groups]
dev = [
    "pytest>=8.3.5",
]
```

`./ws/my-ws-member/pyproject.toml`:
```
[project]
name = "my-ws-member"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = [
    "httpx>=0.28.1",
]

[dependency-groups]
dev = [
    "black>=25.1.0",
]
```

ran commands from root dir: `uv sync`

`uv tree` output:
```
test-ws v0.1.0
├── my-ws-member v0.1.0
│   ├── httpx v0.28.1
│   │   ├── anyio v4.9.0
│   │   │   ├── idna v3.10
│   │   │   └── sniffio v1.3.1
│   │   ├── certifi v2025.1.31
│   │   ├── httpcore v1.0.7
│   │   │   ├── certifi v2025.1.31
│   │   │   └── h11 v0.14.0
│   │   └── idna v3.10
│   └── black v25.1.0 (group: dev)
│       ├── click v8.1.8
│       ├── mypy-extensions v1.0.0
│       ├── packaging v24.2
│       ├── pathspec v0.12.1
│       └── platformdirs v4.3.6
└── pytest v8.3.5 (group: dev)
    ├── iniconfig v2.0.0
    ├── packaging v24.2
    └── pluggy v1.5.0
```

`uv pip freeze`:
```
anyio==4.9.0
certifi==2025.1.31
h11==0.14.0
httpcore==1.0.7
httpx==0.28.1  # <-- workspace member's dependency is included
idna==3.10
iniconfig==2.0.0
packaging==24.2
pluggy==1.5.0
pytest==8.3.5. # <-- workspace root's dev dependency-group is included
sniffio==1.3.1
```

What I would expect is also `black` the dev group-dependency from `my-ws-member` to also be included. And I'm puzzled why it is not

### Platform

macOS Darwin 24.2.0 arm64uv 

### Version

uv 0.6.8 (c1ef48276 2025-03-18)

---

_Label `question` added by @RensDimmendaal on 2025-03-19 13:38_

---

_Renamed from "Why are dev dependency-groups of workspace members not being installed?" to "Question: Why are dev dependency-groups of workspace members not being installed?" by @RensDimmendaal on 2025-03-19 15:08_

---

_Renamed from "Question: Why are dev dependency-groups of workspace members not being installed?" to "Question: why are dev dependency-groups of workspace members not being installed?" by @RensDimmendaal on 2025-03-19 15:08_

---

_Comment by @charliermarsh on 2025-03-19 15:42_

This is intentional. `uv sync` installs the workspace root by default, and so you get the workspace root's dev dependencies. If you want the dev dependency for a member, you should `uv sync -p` that member. Just like you wouldn't want to import one of the member's dependencies from the root package without declaring it as a dependency yourself, you shouldn't be accessing dev dependencies on members like that.



---

_Comment by @charliermarsh on 2025-03-19 15:43_

You can run `uv sync --all-packages` to install the root _and_ all members, which I believe would install all of their dev dependencies too.

---

_Comment by @RensDimmendaal on 2025-03-19 21:46_

@charliermarsh thank you so much for the quick reply. `uv sync --all-packages` was indeed the command that I had missed. Thanks again, I really enjoy using uv.

---

_Closed by @RensDimmendaal on 2025-03-19 21:46_

---
