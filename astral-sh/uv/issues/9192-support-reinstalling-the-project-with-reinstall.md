---
number: 9192
title: "support reinstalling the project with `--reinstall-package`"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2024-11-18T11:12:25Z
updated_at: 2024-11-18T13:18:55Z
url: https://github.com/astral-sh/uv/issues/9192
synced_at: 2026-01-10T01:24:37Z
---

# support reinstalling the project with `--reinstall-package`

---

_Issue opened by @DetachHead on 2024-11-18 11:12_

i would like to be able make `uv sync` always reinstall the current project, because part of my build process involves generating some files that uv is unaware of. this means `uv sync` doesn't always fully install my project

unfortunately the `--reinstall-package` argument doesn't seem to allow this:
```
> uv sync --reinstall-package .
error: invalid value '.' for '--reinstall-package <REINSTALL_PACKAGE>': Not a valid package or extra name: ".". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters
```

---

_Comment by @FishAlchemist on 2024-11-18 12:25_

Can you provide a reproducible pyproject.toml? Since the project needs to be reinstalled, it seems that pyproject.toml should have a ``[build-system]`` section.

e.g.
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
If there's a [build-system] section, then ``<REINSTALL_PACKAGE>`` is the name of the project.

e.g.
pyproject.toml
```toml
[project]
name = "bar"
...
```
```
uv sync --reinstall-package bar
```

I guess this should meet your needs. You can give it a try


---

_Comment by @DetachHead on 2024-11-18 13:18_

that works, thanks! no idea why i didnt think to try that

---

_Closed by @DetachHead on 2024-11-18 13:18_

---
