---
number: 17252
title: "How to use different index only for [dev] group?"
type: issue
state: open
author: phistep
labels:
  - question
assignees: []
created_at: 2025-12-29T15:28:04Z
updated_at: 2026-01-07T10:04:57Z
url: https://github.com/astral-sh/uv/issues/17252
synced_at: 2026-01-07T13:12:19-06:00
---

# How to use different index only for [dev] group?

---

_Issue opened by @phistep on 2025-12-29 15:28_

### Question

I am using `mido` MIDI library. It has only recently added type hints and those have not landed in a stable release. I want normal users to just use the latest stable release, as it supports all features I need at runtime. But for myself, I want to use the latest git rev when developing, so that I get full type checking (with ty ;)

```toml
[project.optional-dependencies]
midi = ["mido[ports-rtmidi]"]

[dependency-groups]
dev = [
    "ruff",
    "ty",
    "mido>=1.3.4", # https://github.com/mido/mido/pull/624
]

[tool.uv.sources]
mido = { git = "https://github.com/mido/mido", group = "dev" }
```

This always pulls in `mido==1.3.4.dev21...` from github

What I want:

- `uv sync --extra midi`: `mido[ports-rtmidi]==1.3.3` (PyPI)
- `uv sync --dev`: `mido==1.3.4.dev12...` (Github)
- `uv sync --dev --extra midi`: `mido[ports-rtmidi]==1.3.4.dev12...` (Github)

How do I configure the `pyproject.toml` correctly?

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.19 (38ee6ec80 2025-07-02)

---

_Label `question` added by @phistep on 2025-12-29 15:28_

---

_Comment by @konstin on 2026-01-06 18:07_

How do users install your library, as a package from PyPI or as a Git dependency?

---

_Comment by @phistep on 2026-01-07 09:41_

As a package from PyPI. t's more of an application than a library actually. I want users to just do `uv tool install vhsh[midi]` from PyPI to get whatever latest `mido[ports-rtmidi]` with their installation. For me as a dev, I want to pull in the latest Git version to do the type checking

---

_Comment by @konstin on 2026-01-07 10:04_

`[tool.uv.sources]` isn't exported/used by PyPI packages, so this should already work. You can test this with `uv build`, then installing the downstream package with `uv pip install --find-links dist/`.

---
