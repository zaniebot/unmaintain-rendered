---
number: 10054
title: Using git versioning with a workspace module yields unwanted changes to uv.lock
type: issue
state: closed
author: wlach
labels: []
assignees: []
created_at: 2024-12-20T11:58:46Z
updated_at: 2024-12-20T14:07:04Z
url: https://github.com/astral-sh/uv/issues/10054
synced_at: 2026-01-10T01:24:49Z
---

# Using git versioning with a workspace module yields unwanted changes to uv.lock

---

_Issue opened by @wlach on 2024-12-20 11:58_

I'm maintaining various libraries inside a monorepo using uv's wonderful [workspaces feature](https://docs.astral.sh/uv/concepts/projects/workspaces/).

Our pyproject.toml looks something like this:

```
[project]
name = "monorepo"
version = "0.1.0"
dependencies = ["mymodule[tests]"]

[tool.uv.workspace]
members = ["src/*"]

[tool.uv.sources]
mymodule = { workspace = true }
```

`mymodule` uses git-based versioning using [setuptools-git-versioning](https://pypi.org/project/setuptools-git-versioning). This gives it a new version like `0.1.post31+git.fc7087bf` which we want for various internal reasons which I won't go into here. Unfortunately since that version is linked to the repository, making *any* change will result in its `uv sync` thinking it needs to update version of the module. The relevant section in `uv.lock` which continuously gets updated is:

```
[[package]]
name = "mymodule"
version = "0.1.post31+git.fc7087bf"
source = { editable = "src/mymodule" }
dependencies = [ ... ]
```

It would be great if we could somehow just skip writing the version for editable modules inside `uv.lock`, since it really shouldn't matter (they're just going to get installed via the filesystem anyway).

---

_Comment by @wlach on 2024-12-20 12:20_

I thought this might be related, but appears not to be (I get the same issue with the latest version of uv, 0.5.11 as of this writing): https://github.com/astral-sh/uv/pull/9549

---

_Comment by @my1e5 on 2024-12-20 13:47_

* Duplicate of https://github.com/astral-sh/uv/issues/7533

---

_Closed by @wlach on 2024-12-20 14:06_

---

_Comment by @wlach on 2024-12-20 14:07_

ah yep, you're totally right

---
