---
number: 15703
title: "`--no-install-local` doesn't appear to be working"
type: issue
state: open
author: sebovzeoueb
labels:
  - question
assignees: []
created_at: 2025-09-05T14:08:49Z
updated_at: 2025-09-10T10:22:01Z
url: https://github.com/astral-sh/uv/issues/15703
synced_at: 2026-01-07T13:12:19-06:00
---

# `--no-install-local` doesn't appear to be working

---

_Issue opened by @sebovzeoueb on 2025-09-05 14:08_

### Summary

I have a project which references packages locally like so:

```
[project]
name = "repro"
version = "0.1.0"
requires-python = ">=3.12, <3.13"
dependencies = [
    "fake-dependency"
]
[tool.uv.sources]
fake-dependency = { path = "/fake_path/", editable = true }
```

The documentation claims that `--no-install-local` "Skips the current project, workspace members, and any other local (path or editable) packages", however I'm getting the following output after running `uv sync --no-install-local`:

`error: Distribution not found at: file:///F:/fake_path`

If I'm understanding the purpose of `--no-install-local` correctly, uv should not attempt to install `fake-dependency` at all. `--no-sources` doesn't fix my use case here, as I want to completely skip the local dependency here and `--no-sources` will attempt to pull it from PyPI where it doesn't exist.

### Platform

Windows 11 64 bit

### Version

0.8.15

### Python version

3.12.10

---

_Label `bug` added by @sebovzeoueb on 2025-09-05 14:08_

---

_Comment by @charliermarsh on 2025-09-06 02:34_

The package still needs to be present to create or validate the lockfile (because the lockfile represents the resolution for the entire project). If you already have a lockfile, and want to sync without `fake-dependency` being present on the machine, then you'd want `uv sync --no-install-local --frozen`. (`--frozen` skips the locking phase.)

---

_Label `bug` removed by @charliermarsh on 2025-09-06 02:35_

---

_Label `question` added by @charliermarsh on 2025-09-06 02:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-06 02:35_

---

_Comment by @sebovzeoueb on 2025-09-09 09:40_

Thanks for your response, I'm migrating to uv from a setup I had using pip and multiple requirements files, your comment about the lockfile is helping me understand some issues I've been running into in general. It may be that there's a better way to achieve what I'm going for anyway. Here are the scenarios I'm trying to cover:

- Production Docker image: `fake-dependency` comes from a package published on PyPI
- Development Docker image: `fake-dependency` comes from `/fake_dependency/` within the Docker container
- Local non-Docker development: `fake-dependency` comes from `../../fake_dependency/`

I have been able to achieve the top 2 with `--no-sources` and `tool.uv.sources` as you can see in my example project.
The only way I've thought of to achieve the 3rd one is by not installing local dependencies and doing a `uv pip install` command to repeat what I was doing with pip before, but this isn't ideal, and also doesn't work, hence the issue I opened. I also don't have a lock file committed in the project as I'm currently having it do the `uv sync` only in Docker and I'm using the watch feature of Docker Compose rather than mounting, so the lock file doesn't get copied back into the repo, which means I shouldn't be able to use `--frozen` as the file doesn't exist. I do feel like I'm doing everything a bit wrong here, any cleaner suggestions as to how to achieve what I'm going for?

---

_Comment by @nbaju1 on 2025-09-10 10:11_

Will this work, installing in the various scenarios using  `uv sync --group <scenario-specific-group>`?

```toml
[project]
name = "repro"
version = "0.1.0"
requires-python = ">=3.12, <3.13"
dependencies = ["<shared dependencies>"]

[dependency-groups]
docker-prod = ["fake-dependency"]
docker-dev = ["fake-dependency-dev"]
local = ["fake-dependency-local"]

[tool.uv.sources]
fake-dependency-dev = { path = "/fake_path/", editable = true }
fake-dependency-local = { path = "../../fake_dependency/", editable = true }
```

Edit: I'm not sure if the dependency names have to match the actual names of the referenced packages. Guess this won't work if they have to align.

Edit 2: This won't work as dependency name has to match the project name. 
```
  × Failed to build `fake-dependency-local @ file:///<redacted>/fake-dependency`
  ╰─▶ Package metadata name `fake-dependency` does not match given name `fake-dependency-local`

---
