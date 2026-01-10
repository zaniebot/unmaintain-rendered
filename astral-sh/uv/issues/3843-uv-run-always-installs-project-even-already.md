---
number: 3843
title: "`uv run` always installs project even already installed in editable mode"
type: issue
state: closed
author: Vigilans
labels: []
assignees: []
created_at: 2024-05-26T19:14:25Z
updated_at: 2024-05-26T19:17:18Z
url: https://github.com/astral-sh/uv/issues/3843
synced_at: 2026-01-10T01:23:31Z
---

# `uv run` always installs project even already installed in editable mode

---

_Issue opened by @Vigilans on 2024-05-26 19:14_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

* uv version: 0.2.3

I have setup a python project with `pyproject.yaml`, and installed it in editable mode using `uv pip install -e .`. 

When invoking `uv run`, it always installs the project in `build` directory:

```
$ uv run ls                                                                                                                                      
warning: `uv run` is experimental and may change without warning.
Resolved 40 packages in 14ms
Uninstalled 1 package in 0.59ms
Installed 1 package in 23ms
 - <project_name>==1.0.0 (from file:///workspace)
 + <project_name>==1.0.0 (from file:///home/vigilans/...)
```

And `resolved` / `audited` log always pop up in all `uv run` executions:
```
$ uv run echo                                                                                                                                    
warning: `uv run` is experimental and may change without warning.
Resolved 40 packages in 16ms
Audited 40 packages in 0.05ms
```

If `.venv` folder is not writable, the log will be
```
$ uv run ls                                                                                                                                      
warning: `uv run` is experimental and may change without warning.
Resolved 40 packages in 415ms
error: failed to remove file `/workspace/.venv/lib/python3.7/site-packages/__editable__.<project_name>-1.0.0.pth`
  Caused by: Permission denied (os error 13)
```
Seems it tries to revert the editable mode and install in dist mode (what `Uninstall 1 packages` and `Install 1 packages` in first log is).

It would be better if installation in editable mode also works for `uv run`.

---

_Comment by @charliermarsh on 2024-05-26 19:17_

This is covered by #3628. Thanks!

---

_Closed by @charliermarsh on 2024-05-26 19:17_

---
