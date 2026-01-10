```yaml
number: 17676
title: "Ruff formats and checks `.venv` files when repository is not a Git repo"
type: issue
state: closed
author: AliHezarpisheh
labels:
  - needs-info
assignees: []
created_at: 2025-04-28T08:22:00Z
updated_at: 2025-05-12T08:23:12Z
url: https://github.com/astral-sh/ruff/issues/17676
synced_at: 2026-01-10T11:09:58Z
```

# Ruff formats and checks `.venv` files when repository is not a Git repo

---

_Issue opened by @AliHezarpisheh on 2025-04-28 08:22_

### Summary
When using `ruff` in a project folder that has **no Git initialized** (no `.git` directory), running `ruff format` or `ruff check` causes it to process files inside the `.venv/` directory.  
However, once `git init` is run in the folder, `ruff` correctly ignores `.venv/` and behaves as expected.

### Steps to Reproduce
1. Create a new folder (no Git repo).
2. Set up a Python virtual environment (`python -m venv .venv`).
3. Install `ruff`.
4. Run `ruff check .` or `ruff format .`
5. Observe that `ruff` checks or formats files inside `.venv/`.

Now:
6. Run `git init` inside the folder.
7. Run `ruff check .` or `ruff format .` again.
8. Observe that `.venv/` is ignored.

ruff version: 0.11.7

---

_Comment by @MichaReiser on 2025-04-28 08:30_

Could you share your configuration? 

Ruff should skip `.venv` unless you override the [`exclude`](https://docs.astral.sh/ruff/settings/#exclude) setting.

---

_Label `needs-info` added by @MichaReiser on 2025-04-28 08:30_

---

_Closed by @MichaReiser on 2025-05-12 08:23_

---
