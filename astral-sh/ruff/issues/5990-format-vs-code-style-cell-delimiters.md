---
number: 5990
title: "Format VS Code-style `# %%` cell delimiters"
type: issue
state: open
author: janosh
labels:
  - needs-decision
assignees: []
created_at: 2023-07-22T21:55:58Z
updated_at: 2023-07-24T02:23:03Z
url: https://github.com/astral-sh/ruff/issues/5990
synced_at: 2026-01-10T01:22:45Z
---

# Format VS Code-style `# %%` cell delimiters

---

_Issue opened by @janosh on 2023-07-22 21:55_

VS Code Python has a very nice feature called [Interactive Window](https://code.visualstudio.com/docs/python/jupyter-support-py) that allows to run regular Python scripts in a Jupyter-like interface.

![code-cells-01](https://github.com/astral-sh/ruff/assets/30958850/4bda9371-ff9b-40a8-8c15-c79cece6a0a4)

Cells are delimited by `# %%`. For readability, I like to keep 2 blank lines above every cell delimiter (except if at the top of a file).
I wrote [`format-ipy-cells`](https://github.com/janosh/format-ipy-cells) to auto-format that as well as delete empty cells and enforce comment spacing when on the same line as a cell delimiter.

Are these rules that could be integrated into `ruff`?

---

_Label `needs-decision` added by @charliermarsh on 2023-07-24 02:23_

---
