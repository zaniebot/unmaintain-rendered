```yaml
number: 5258
title: "Expose more fine-grained control for `uv sync` command"
type: issue
state: closed
author: Vigilans
labels: []
assignees: []
created_at: 2024-07-21T14:13:00Z
updated_at: 2024-07-31T13:54:18Z
url: https://github.com/astral-sh/uv/issues/5258
synced_at: 2026-01-10T04:53:49Z
```

# Expose more fine-grained control for `uv sync` command

---

_Issue opened by @Vigilans on 2024-07-21 14:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv sync` can be roughly divided into three steps:
1. Sync venv (uses `project.requires-python` in pyproject.toml)
2. Sync dependencies (uses `project.dependencies` in pyproject.toml)
3. Install project

User may demand more specific behaviors in the sync process. For example:
1. User may only want to sync venv. (Create a venv based on current project's `project.requires-python` in pyproject.toml)
2. User may only want to sync venv and install the dependency, but do not install the project. (https://github.com/astral-sh/uv/issues/4028, https://github.com/astral-sh/uv/issues/1516)
3. User may want to fully sync the environment, but install the project in editable mode.

---
Example 2 and 3 can be achieved with workaround if we have 1:
2. Use `uv pip install -r pyproject.toml` on the synced venv.
3. Use `uv pip install -e .` on the synced venv.

I would like to know do we have a way to only create venv based on `requires-python` in pyproject.toml?

---

_Comment by @zanieb on 2024-07-21 17:46_

To respond in part: I think, in the future, `uv venv` will respect the `pyproject.toml` by default.

---

_Closed by @zanieb on 2024-07-31 13:54_

---

_Closed by @zanieb on 2024-07-31 13:54_

---
