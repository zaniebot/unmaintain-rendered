---
number: 7071
title: Workspace exclusion does not work
type: issue
state: closed
author: blinkseb
labels:
  - bug
assignees: []
created_at: 2024-09-05T06:55:51Z
updated_at: 2024-09-09T16:08:07Z
url: https://github.com/astral-sh/uv/issues/7071
synced_at: 2026-01-07T13:12:17-06:00
---

# Workspace exclusion does not work

---

_Issue opened by @blinkseb on 2024-09-05 06:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv platform: Fedora 40
uv version: `uv 0.4.5`

Hello,

First of all, thanks a lot for `uv`, it's an awesome tool!

I'm trying to migrate my current project from `poetry` to `uv` by leveraging the workspace functionality. I have the following projects structure:

```
.
├── projects
│   ├── a
│   │   ├── hello.py
│   │   ├── pyproject.toml
│   │   └── README.md
│   ├── b
│   │   ├── ba
│   │   │   ├── hello.py
│   │   │   ├── pyproject.toml
│   │   │   └── README.md
│   │   └── bb
│   │       ├── hello.py
│   │       ├── pyproject.toml
│   │       └── README.md
│   └── c
│       ├── hello.py
│       ├── pyproject.toml
│       └── README.md
├── pyproject.toml
```

The root `pyproject.toml`, the workspace root, is defined as:

```toml
[project]
name = "project"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.12"
dependencies = []

[tool.uv]
package = false

[tool.uv.workspace]
members = ["projects/*", "projects/b/*"]
exclude = ["projects/b"]
```

However, `uv lock` (or basically any `uv` command) fails this the following error message:

```
❯ uv lock
error: Workspace member `/home/sbrochet/uv-test/workspace/projects/b` is missing a `pyproject.toml` (matches: `projects/*`)
```

even if `projects/b` is explicitly excluded.

I've also tried to remove the members entry `projects/b/*`, but it fails with the same error message.

Thanks a lot again!

---

_Comment by @charliermarsh on 2024-09-05 13:17_

Thanks, this looks like a bug. I'll take a look today.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-05 13:17_

---

_Label `bug` added by @charliermarsh on 2024-09-05 13:17_

---

_Comment by @charliermarsh on 2024-09-05 13:20_

To clarify, do you want `projects/a/ba` to be included? Or everything under `projects/b` should be excluded?

---

_Comment by @blinkseb on 2024-09-05 13:59_

> Thanks, this looks like a bug. I'll take a look today.

Awesome thanks a lot!

> To clarify, do you want `projects/a/ba` to be included? Or everything under `projects/b` should be excluded?

Ideally I'd like to have `projects/b/ba` included in the workspace since only `projects/b` is excluded. 

---

_Comment by @charliermarsh on 2024-09-05 14:42_

No prob. Is `projects/b` a project? Like does `projects/b/pyproject.toml` exist? Or it's just a directory that contains other projects? (I assume you also want `projects/b/bb` to be included?)


---

_Comment by @blinkseb on 2024-09-06 06:38_

I'm sorry, I see how my example can be confusing. We have a monorepo where we have multiple projects stored in the `projects` folder. We then sub-categorize the projects per product, each into their own sub-folder. We also have cross-product project which sit at the root of the `projects` folder. So a more realistic layout would be the following

```
.
├── projects
│   ├── product-a
│   │   └── project
│   │       └── pyproject.toml
│   ├── product-b
│   │   └── project
│   │       └── pyproject.toml
│   └── project-a
│       └── pyproject.toml
├── pyproject.toml
```

Here `product-a` and `product-b` are only used to group projects together, they are not python project and do no have a `pyproject.toml` file. So I would except that the following `uv` `pyproject.toml` to work:

```
[tool.uv.workspace]
members = ["projects/*", "projects/product-a/*", "projects/product-b/*"]
exclude = ["projects/product-a", "projects/product-b"]
```

---

_Comment by @charliermarsh on 2024-09-07 00:30_

Ok thank you, I think I follow now. I just tested and it looks like Cargo (which we modeled our workspace discovery after) doesn't support this kind of interaction either. It makes sense that it doesn't work if you consider that excludes are applied after member discovery, and we discover by walking over directories. Though I'd argue that the error message is wrong:

```
error: Workspace member `/Users/crmarsh/workspace/uv/projects/projects/product-a` is missing a `pyproject.toml` (matches: `projects/*`)
```

It should instead say that we can't find a workspace member for `product-a` if it's referenced somewhere else, or similar. We shouldn't be trying to find a project in `projects/product-a`, since it's excluded.

I think you'll need a slightly different structure to make this work...

One option is to avoid including `"projects/*"`, like you could do:

```toml
[tool.uv.workspace]
members = ["projects/project-a", "projects/product-a/*", "projects/product-b/*"]
```

Or:

```toml
[tool.uv.workspace]
members = ["projects/project-*", "projects/product-a/*", "projects/product-b/*"]
```

(I assume they don't actually follow that pattern.)

Or you could put all the projects in a subdirectory of their own, like `projects/projects/project-a`?


---

_Label `question` added by @charliermarsh on 2024-09-07 00:30_

---

_Referenced in [astral-sh/uv#7175](../../astral-sh/uv/pulls/7175.md) on 2024-09-07 18:32_

---

_Comment by @charliermarsh on 2024-09-07 18:33_

Ok, this layout actually works after #7175:

```toml
[project]
name = "projects"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.12"
dependencies = []

[tool.uv]
package = false

[tool.uv.workspace]
members = ["projects/*", "projects/product-a/*", "projects/product-b/*"]
exclude = ["projects/product-a", "projects/product-b"]
```

---

_Label `question` removed by @charliermarsh on 2024-09-07 18:33_

---

_Closed by @charliermarsh on 2024-09-09 16:08_

---

_Closed by @charliermarsh on 2024-09-09 16:08_

---
