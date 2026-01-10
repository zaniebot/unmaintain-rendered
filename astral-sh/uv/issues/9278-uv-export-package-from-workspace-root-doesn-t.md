---
number: 9278
title: "`uv export --package` from workspace root doesn't export root's dependencies"
type: issue
state: closed
author: ultrapoci
labels:
  - question
assignees: []
created_at: 2024-11-20T15:35:39Z
updated_at: 2024-11-20T20:20:34Z
url: https://github.com/astral-sh/uv/issues/9278
synced_at: 2026-01-10T01:24:38Z
---

# `uv export --package` from workspace root doesn't export root's dependencies

---

_Issue opened by @ultrapoci on 2024-11-20 15:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I have a workspace setup with two subdirectories. The `pyproject.toml` file in the workspace root contains a dependency, which is then available for all projects in the workspace. In fact, if I run `uv run --package package1 pacakge1/hello.py`, it runs correctly when importing said dependency. 
The problem is that, if I run `uv export --package package1`, I get an empty result, because the `pyproject.toml` file in package1 is actually empty, but in reality package1 can use the dependencies declared in `pyproject.toml` in the workspace root.
Shouldn't `uv export` include all dependencies from the project root plus the dependencies for the project inside the workspace, if the `--package` flag is used?

uv version: 0.5.3 (using on macOS)
command invoked: `uv export --no-hashes --package package1`


---

_Comment by @charliermarsh on 2024-11-20 15:37_

I think this is working as intended. `uv export --package package1` exports the dependency tree starting at `package1`. `package1` shouldn't be importing dependencies that are defined in the workspace root, but not declared as dependencies of `package1`.

---

_Comment by @ultrapoci on 2024-11-20 15:43_

I understand. To solve this, I've explicitly added to package1 a dependency on the workspace root: `uv add . --package package1`. Does this approach have any drawbacks? 

---

_Comment by @charliermarsh on 2024-11-20 16:01_

No, that's a reasonable approach!

---

_Label `question` added by @charliermarsh on 2024-11-20 16:01_

---

_Closed by @charliermarsh on 2024-11-20 20:20_

---
