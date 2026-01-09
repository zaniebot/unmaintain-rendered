---
number: 6499
title: Add Support for Script Execution via Configuration File
type: issue
state: closed
author: CucumberKing
labels: []
assignees: []
created_at: 2024-08-23T08:14:12Z
updated_at: 2025-07-14T03:00:52Z
url: https://github.com/astral-sh/uv/issues/6499
synced_at: 2026-01-07T13:12:17-06:00
---

# Add Support for Script Execution via Configuration File

---

_Issue opened by @CucumberKing on 2024-08-23 08:14_

Could you introduce an option in `uv` to allow running scripts defined in a configuration file, similar to the functionality provided by PDM and Rye.

### Background

Both [PDM](https://pdm-project.org/en/latest/usage/scripts/) and [Rye](https://rye.astral.sh/guide/pyproject/#toolryesources) offer functionality to define and run custom scripts directly from their configuration files. This feature simplifies the development process by enabling the execution of common tasks (e.g., testing, building, deploying) through a unified command interface.

For example:
- **PDM** uses the `[tool.pdm.scripts]` section in the `pyproject.toml` file to define scripts.
- **Rye** allows script definitions under `[tool.rye.scripts]` in the `pyproject.toml`.

### Proposal

Add similar functionality to `uv` that allows users to define scripts in a configuration file (e.g., `pyproject.toml`) and execute them via a simple `uv` command.

### Benefits

- **Consistency**: Users familiar with tools like PDM and Rye will have a consistent experience.
- **Convenience**: Simplifies the process of running repetitive tasks, improving developer productivity.
- **Automation**: Encourages the automation of common project tasks, reducing manual overhead.

### Suggested Implementation

1. **Configuration**: Introduce a section in the `pyproject.toml` (or equivalent config file) under `[tool.uv.scripts]` where users can define scripts.

   Example:
   ```toml
   [tool.uv.scripts]
   test = "pytest tests/"
   build = "python setup.py sdist bdist_wheel"
   ```

2. **Command Execution**: Add a command to `uv` that can execute these scripts. For example:
   ```
   uv run test
   uv run build
   ```


---

_Comment by @my1e5 on 2024-08-23 08:24_

Duplicate of https://github.com/astral-sh/uv/issues/5903

---

_Closed by @CucumberKing on 2024-08-23 08:31_

---

_Comment by @CucumberKing on 2024-08-23 08:31_

did not see the duplicate

---

_Comment by @wuchang on 2025-07-14 03:00_

这个功能是非常需要

---
