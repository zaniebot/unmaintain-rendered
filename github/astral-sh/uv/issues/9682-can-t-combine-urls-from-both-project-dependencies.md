---
number: 9682
title: "\"Can't combine URLs from both `project.dependencies` and `tool.uv.sources`\" when combining GitHub link and local path"
type: issue
state: closed
author: bersbersbers
labels:
  - duplicate
assignees: []
created_at: 2024-12-06T13:38:24Z
updated_at: 2024-12-09T17:16:09Z
url: https://github.com/astral-sh/uv/issues/9682
synced_at: 2026-01-07T13:12:18-06:00
---

# "Can't combine URLs from both `project.dependencies` and `tool.uv.sources`" when combining GitHub link and local path

---

_Issue opened by @bersbersbers on 2024-12-06 13:38_

I would like to support editable installs of a package and one of its dependencies, if the dependencies are already checked out locally.

This is what I tried:

```toml
[project]
name = "Test"
version = "0.1"
dependencies = [
    "humanize @ git+https://github.com/python-humanize/humanize.git/",
]

[tool.uv.sources]
humanize = { path = "../humanize", editable = true }
```

Unfortunately, `uv pip install .` gives me 

```
  × Failed to build `test @ file:///C:/Git/bug`
  ├─▶ Failed to parse entry: `humanize`
  ╰─▶ Can't combine URLs from both `project.dependencies` and `tool.uv.sources`
```

I read some [documentation](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-sources), but I must admit I was unable to figure out if the above should be supported or not.

```
uv 0.5.6 (b70c4f30e 2024-12-03)
```

---

_Comment by @FishAlchemist on 2024-12-06 13:50_

This issue(https://github.com/astral-sh/uv/issues/7945) mentions your problem, so I think you should discuss it there.
https://github.com/astral-sh/uv/issues/7945#issuecomment-2409866279

---

_Comment by @charliermarsh on 2024-12-06 14:25_

@bersbersbers -- What do you want / expect to happen when someone runs `uv pip install .` on that project?

---

_Label `duplicate` added by @zanieb on 2024-12-06 14:27_

---

_Comment by @bersbersbers on 2024-12-07 00:14_

@charliermarsh that's a good question, since the documentation is rather sparse on this feature (unless I looked in the wrong place).

I think I would _expect_ to happen whatever happens for this `pyproject.toml` (where the "main" source is now PyPI rather than GitHub):
```toml
[project]
name = "Test"
version = "0.1"
dependencies = ["humanize"]  # from PyPI

[tool.uv.sources]
humanize = { path = "../humanize", editable = true }
```

In an ideal world, I would _want_ `uv pip install -e .` to install "Test" and "humanize" in editable mode, from `.` and `../humanize`, respectively.

I am unsure what `uv pip install .` should do for "humanize". I think both options would be fine for me (GitHub or `../humanize`), with a slight preference for the local editable install. I would like if the local install folder was optional: meaning, if it was missing, it should not impact installation of the package, at least in non-editable mode - maybe also generally.

---

_Referenced in [astral-sh/uv#9718](../../astral-sh/uv/pulls/9718.md) on 2024-12-08 13:45_

---

_Closed by @charliermarsh on 2024-12-09 17:16_

---

_Closed by @charliermarsh on 2024-12-09 17:16_

---
