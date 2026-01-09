---
number: 14891
title: Install entry points for workspace members
type: issue
state: closed
author: tuukkamustonen
labels:
  - question
assignees: []
created_at: 2025-07-25T12:13:20Z
updated_at: 2025-07-25T13:05:06Z
url: https://github.com/astral-sh/uv/issues/14891
synced_at: 2026-01-07T13:12:19-06:00
---

# Install entry points for workspace members

---

_Issue opened by @tuukkamustonen on 2025-07-25 12:13_

### Summary

[Entry points](https://docs.astral.sh/uv/concepts/projects/config/#entry-points) don't seem to get installed for the nested packages when developing in a [workspace](https://docs.astral.sh/uv/concepts/projects/workspaces/).

Tried to configure a [commitizen entry point](https://commitizen-tools.github.io/commitizen/customization/#migrating-from-legacy-plugin-format) in a nested package, e.g. `packages/foobar/pyproject.toml`:

```toml
[project.entry-points.'commitizen.plugin']
cz_custom = "my_pkg:CustomCz"
```

Then `uv sync` on the main level and:

```
python -c "import importlib.metadata; importlib.metadata.entry_points(group='commitizen.plugin')"
```

...but it doesn't show the added entry point. Also tried with a [script entry point](https://docs.astral.sh/uv/concepts/projects/config/#command-line-interfaces), but doesn't work with that either.

It _does work_, if I define the entry point _at the main level_ `pyproject.toml`, but not in a workspace member.

And it also works, if I `cd packages/foobar` and `uv sync` there (removes libs of other packages, so not a workaround).

### Platform

Kubuntu 24.04 amd64

### Version

0.8.2

### Python version

Python 3.10.18

---

_Label `bug` added by @tuukkamustonen on 2025-07-25 12:13_

---

_Comment by @zanieb on 2025-07-25 12:54_

Is your workspace member a dependency of the workspace root?

---

_Label `bug` removed by @zanieb on 2025-07-25 12:54_

---

_Label `question` added by @zanieb on 2025-07-25 12:54_

---

_Comment by @tuukkamustonen on 2025-07-25 13:04_

Oh crap, that's the one place I forgot it from 😮‍💨 Ie.

```toml
dependencies = [
    ...was missing from here...
]
```

Works _perfectly_, sorry for noise!

---

_Closed by @tuukkamustonen on 2025-07-25 13:04_

---
