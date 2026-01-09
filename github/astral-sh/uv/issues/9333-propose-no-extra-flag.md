---
number: 9333
title: "Propose `--no-extra` Flag"
type: issue
state: closed
author: paultreadel
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-11-21T18:04:01Z
updated_at: 2024-11-27T21:29:20Z
url: https://github.com/astral-sh/uv/issues/9333
synced_at: 2026-01-07T13:12:18-06:00
---

# Propose `--no-extra` Flag

---

_Issue opened by @paultreadel on 2024-11-21 18:04_

The new conflicting depencies handling introduced in 0.5.3 is great.  Able to solve several project challenges with it.

To enhance the feature I'd propose adding a `--no-extra` flag allowing a specific extra group to be excluded when used with `--all-extras`.

With the config below running `uv sync --all-extras`  results in an error.

```
Resolved 3 packages in 1ms
error: extra `extra1`, extra `extra2` are incompatible with the declared conflicts: {`uv-conflicts[extra1]`, `uv-conflicts[extra2]`}
```
I'd propose one option for handling this to allow the user to specify which of the conflicting extras to exclude.  The option for this syntax could be helpful when the project has many optional extras where specifying each individual could become a long chain.

`uv sync --all-extras --no-extra extra2`

Similar functionality already exists for dependency groups with `uv sync --all-groups --no-groups <NO_GROUP>` and this could behave in a similar manner for extras as well.

```toml
[project]
name = "uv-conflicts"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
extra1 = ["numpy==2.1.2"]
extra2 = ["numpy==2.0.0"]

[tool.uv]
conflicts = [
    [
      { extra = "extra1" },
      { extra = "extra2" },
    ],
]
```

---

_Comment by @charliermarsh on 2024-11-21 18:46_

I think this makes sense given that `--no-group` exists \cc @zanieb 

---

_Label `enhancement` added by @charliermarsh on 2024-11-22 13:59_

---

_Label `help wanted` added by @charliermarsh on 2024-11-22 13:59_

---

_Referenced in [astral-sh/uv#9387](../../astral-sh/uv/pulls/9387.md) on 2024-11-23 16:24_

---

_Closed by @charliermarsh on 2024-11-24 02:25_

---

_Closed by @charliermarsh on 2024-11-24 02:25_

---

_Comment by @paultreadel on 2024-11-27 21:29_

Thanks for the prompt turnaround on the new feature!  Already using it in v0.5.5 and its working great for handling conflicting extras and simplifying the commands required to do so.

---
