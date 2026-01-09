---
number: 7747
title: Jupyter Notebook outputs raw source code upon fixing via stdin
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
created_at: 2023-10-01T20:24:07Z
updated_at: 2023-10-02T14:20:14Z
url: https://github.com/astral-sh/ruff/issues/7747
synced_at: 2026-01-07T13:12:15-06:00
---

# Jupyter Notebook outputs raw source code upon fixing via stdin

---

_Issue opened by @charliermarsh on 2023-10-01 20:24_

If you run, e.g., `cat Untitled.ipynb | cargo run -p ruff_cli -- check --stdin-filename Untitled.ipynb -n --fix`, then the content we write to `stdout` is the fixed source code. Writing this _back_ to the notebook will leave you with an invalid notebook! Instead, we need to write back the entire JSON structure, including the fixed content.

---

_Label `bug` added by @charliermarsh on 2023-10-01 20:24_

---

_Label `cli` added by @charliermarsh on 2023-10-01 20:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-01 20:30_

---

_Referenced in [astral-sh/ruff#7748](../../astral-sh/ruff/pulls/7748.md) on 2023-10-01 20:44_

---

_Closed by @charliermarsh on 2023-10-02 14:20_

---
