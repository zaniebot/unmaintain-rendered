---
number: 15697
title: Create a lockfile for a workspace member
type: issue
state: closed
author: fau-st
labels:
  - question
assignees: []
created_at: 2025-09-05T09:05:54Z
updated_at: 2025-09-05T10:00:10Z
url: https://github.com/astral-sh/uv/issues/15697
synced_at: 2026-01-10T01:25:58Z
---

# Create a lockfile for a workspace member

---

_Issue opened by @fau-st on 2025-09-05 09:05_

### Question

Hello,

Given the following sample project structure:
```
my_app
├── main.py
├── my_lib
│   ├── pyproject.toml
│   └── src
│       └── my_lib
│           ├── __init__.py
│           └── py.typed
├── pyproject.toml
└── uv.lock
```
```toml
[project]
name = "my-app"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "my-lib",
]

[tool.uv.sources]
my-lib = { workspace = true }

[tool.uv.workspace]
members = [
    "my_lib",
]
```
Is there a way to generate/update `uv.lock` for `my_lib`?

### My use case

In our real project, `my_app/my_lib` is a git submodule that lives in another repository.
This repo has some CI scripts that rely on the `uv.lock` to ensure a reproducible build.

The issue is: when a developer works on `my_app`, they sometime need to add a new dependency to `my_lib` so they `cd my_app/my_lib` and then `uv add ...` because it's more convenient than cloning the `my_lib` repo and do it there.
Doing this properly updates `my_app/uv.lock` and `my_app/my_lib/pyproject.toml` however `my_app/my_lib/uv.lock` is left as-is.

Right now, the only "workaround" I found is to rename `my_app/pyproject.toml` to something else so I can `cd my_app/my_lib && uv lock`.

Would there be a way to ignore that `my_app/my_lib` is inside a workspace for the duration of a single command, so I could do something like `cd my_app/my_lib && uv lock --ignore-workspace`?
Or am I trying to do something outside of the workspace feature scope?

Kind of related [issue](https://github.com/astral-sh/uv/issues/9150) but I don't really care about a separate venv, I'd just like to update the lockfile of a workspace member.

Thank you very much for the great tools you're building! I really enjoy using them.


### Platform

Ubuntu 24.04

### Version

0.8.15

---

_Label `question` added by @fau-st on 2025-09-05 09:05_

---

_Comment by @konstin on 2025-09-05 09:38_

Workspaces are built under the assumption that everything lives in one repo and uses one lockfile. Does a path dependency on my-lib work for you too? That way, both projects get a lockfile in their respective repostories.

---

_Comment by @fau-st on 2025-09-05 09:54_

Yes I already tried using a path dependency with `editable = true` and indeed I get a lockfile for the member but I lose some nice features (`uv run --package`, `uv build --all-packages`, etc...), but well, it's documented.
Maybe I'm being too greedy by wanting the best of both worlds for my use case 😅
If there's nothing more that can be done, tell me, I'll just close this issue.

---

_Comment by @konstin on 2025-09-05 09:57_

One other option may be using the `pylock.toml` export for my-lib, but apart from that I'm afraid a path dependency is the best we can do there currently.

---

_Comment by @fau-st on 2025-09-05 10:00_

Ok, that's what I wanted to know. Thanks!

---

_Closed by @fau-st on 2025-09-05 10:00_

---
