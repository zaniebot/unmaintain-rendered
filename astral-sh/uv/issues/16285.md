```yaml
number: 16285
title: "Workspace discovery fails when there is a leading `./` in `tool.uv.workspace.members`"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2025-10-13T15:56:09Z
updated_at: 2025-10-14T17:15:20Z
url: https://github.com/astral-sh/uv/issues/16285
synced_at: 2026-01-10T03:23:54Z
```

# Workspace discovery fails when there is a leading `./` in `tool.uv.workspace.members`

---

_Issue opened by @konstin on 2025-10-13 15:56_

### Summary

To reproduce, create a `pyproject.toml` workspace root:

```toml
[tool.uv.workspace]
members = []
```

Then create to members with:

```
uv init foo
uv init bar
```

In the `bar` directory, do:

```
uv add foo
```

Now, `pyproject.toml` has both projects:

```toml
[tool.uv.workspace]
members = [
    "foo",
    "bar",
]
```

Run `uv sync`, see that the workspace is correctly used:

```
$ uv sync -v
[...]
DEBUG Found project root: `/home/konsti/projects/reproducer`
DEBUG Adding discovered workspace member: `/home/konsti/projects/reproducer/foo`
DEBUG Adding discovered workspace member: `/home/konsti/projects/reproducer/bar`
[...]
```

Now, add leading `./` to the member globs:

```toml
[tool.uv.workspace]
members = [
    "./foo",
    "./bar",
]
```

Now `uv sync` fails:

```
$ uv sync
error: Failed to generate package metadata for `bar==0.1.0 @ virtual+bar`
  Caused by: Failed to parse entry: `foo`
  Caused by: `foo` references a workspace in `tool.uv.sources` (e.g., `foo = { workspace = true }`), but is not a workspace member
```

This problem becomes clearer when switching to the `bar` directory: We're discovering the workspace and its members from the root, but not from a member:

```
$ uv sync -v
[...]
DEBUG Found project root: `/home/konsti/projects/reproducer/bar`
DEBUG Found workspace root `/home/konsti/projects/reproducer`, but project is not included
DEBUG No workspace root found, using project root
[...]
error: Failed to generate package metadata for `bar==0.1.0 @ virtual+.`
  Caused by: Failed to parse entry: `foo`
  Caused by: `foo` references a workspace in `tool.uv.sources` (e.g., `foo = { workspace = true }`), but is not a workspace member
```

CC @RonnyPfannschmidt who discovered this

### Platform

Ubuntu 24.04 amd64

### Version

uv 0.9.2

### Python version

3.12.3

---

_Label `bug` added by @konstin on 2025-10-13 15:56_

---

_Closed by @konstin on 2025-10-14 17:15_

---
