```yaml
number: 17376
title: "uv sync --no-install-workspace fails on 'invalid' workspaces"
type: issue
state: open
author: kurtsys-vintecc
labels:
  - question
assignees: []
created_at: 2026-01-09T13:24:46Z
updated_at: 2026-01-09T14:07:03Z
url: https://github.com/astral-sh/uv/issues/17376
synced_at: 2026-01-12T16:02:49Z
```

# uv sync --no-install-workspace fails on 'invalid' workspaces

---

_@kurtsys-vintecc_

### Summary

## Description

Running a command like this in a Dockerfile:
```
RUN --mount=type=cache,target=$HOME/.cache/uv,uid=... \
    --mount=type=bind,source=pyproject.toml,target=./pyproject.toml \
  uv sync --active --no-config \
    --no-install-workspace \
    --group dev
```
with `pyproject.toml`:
```
[tool.uv.sources]
base-workspace = { workspace = true }
...

[tool.uv.workspace]
members = [
  "python/base_workspace"
  ...
]
```

The command fails since the workspaces are not yet mounted in the container being built:

```
 => ERROR [dev_container_auto_added_stage_label 20/20] RUN --mount=type=c  0.2s
------
 > [dev_container_auto_added_stage_label 20/20] RUN --mount=type=cache,target=.../.cache/uv,uid=1000     --mount=type=bind,source=pyproject.toml,target=./pyproject.toml     --uv sync --active --no-config     --no-install-workspace     --group dev:
0.179   × Failed to build `...`
0.179   ├─▶ Failed to parse entry in group `local`: `...`
0.179   ╰─▶ `...` references a workspace in `tool.uv.sources` (e.g.,
0.179       `... = { workspace = true }`), but is not a workspace
0.179       member
------
```

## Expected behaviour

When `--no-install-workspace` is used, I'd expect `uv sync` to skip workspace configurations, also when parsing pyproject.toml. Even if the workspaces are not available (yet), this shouldn't be an issue, since we add the flag `--no-install-workspace`.


## Workaround

Make sure to have a lock file, and use `--frozen`.

```
RUN --mount=type=cache,target=$HOME/.cache/uv,uid=... \
    --mount=type=bind,source=pyproject.toml,target=./pyproject.toml \
    --mount=type=bind,source=uv.lock,target=./uv.lock \
    uv sync --frozen --active --no-config \
    --no-install-workspace \
    --group dev
```


### Platform

Ubuntu 24.04.3 LTS

### Version

uv 0.9.22

### Python version

Python 3.12.12

---

_Label `bug` added by @kurtsys-vintecc on 2026-01-09 13:24_

---

_Comment by @zanieb on 2026-01-09 13:46_

It's correct to require `--frozen` here. `--no-install-workspace` means we won't install that subset of the lockfile, but we still need to resolve the entire lockfile which includes those workspace members.

---

_Label `bug` removed by @zanieb on 2026-01-09 13:47_

---

_Label `question` added by @zanieb on 2026-01-09 13:47_

---

_Comment by @kurtsys-vintecc on 2026-01-09 14:01_

Right, but why would one generate the entire lockfile, if explicitly the `--no-install-workspace` flag is added? We don't need those workspace members, if we say we don't want them? :) 

---

_Comment by @zanieb on 2026-01-09 14:07_

The workspace members can affect the resolution.

We don't support partial locks, e.g., we also solve for all dependency groups even if you are only installing one of them. That's just a design constraint of the lockfile.

---
