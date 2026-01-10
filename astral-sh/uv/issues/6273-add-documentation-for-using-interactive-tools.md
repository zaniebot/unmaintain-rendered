---
number: 6273
title: "Add documentation for using interactive tools with `uv run`"
type: issue
state: open
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2024-08-20T20:23:36Z
updated_at: 2024-08-23T09:20:02Z
url: https://github.com/astral-sh/uv/issues/6273
synced_at: 2026-01-10T01:23:57Z
---

# Add documentation for using interactive tools with `uv run`

---

_Issue opened by @zanieb on 2024-08-20 20:23_

e.g., in a project,

```
uv run --with ipython ipython
```

or, with `uvx` (#6272)

```
uvx --with . ipython
```

or, without a project,

```
❯ uvx --with requests ipython
Installed 21 packages in 53ms
Python 3.12.1 (main, Jan  7 2024, 23:31:12) [Clang 16.0.3 ]
Type 'copyright', 'credits' or 'license' for more information
IPython 8.26.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: ...
```

---

_Label `documentation` added by @zanieb on 2024-08-20 20:24_

---

_Comment by @telenieko on 2024-08-23 09:20_

What about without a project but with inline metadata?

like: https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies

---
