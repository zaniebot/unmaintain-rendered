```yaml
number: 16249
title: "Add `--no-wait` argument for the `run` command to not wait for spawned process to finish"
type: pull_request
state: open
author: CrendKing
labels: []
assignees: []
base: main
head: main
created_at: 2025-10-11T05:19:06Z
updated_at: 2025-10-22T19:27:54Z
url: https://github.com/astral-sh/uv/pull/16249
synced_at: 2026-01-12T16:12:11Z
```

# Add `--no-wait` argument for the `run` command to not wait for spawned process to finish

---

_@CrendKing_

Here is an unsolicited attempt to implement the feature to `uv run` a script and make `uv` exit immediately without waiting for the spawned `python` process to finish.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I usually need to wrap a bunch of `uv` invocations in other Python scripts. Since `uv` always waits for child process to finish, this ends up with a lot of recursive `uv` processes lingering. Sometimes this is useful, so that logic that must happen after the child process can run at right time. Other times this is unnecessary/wasteful because all I need is to let `uv` to spawn the virtual env. The child script will handle everything by themselves.

This is a very rough initial attempt to implement `uv run --no-wait`. The PR basically consists of adding and passing the argument, then if user needs `--no-wait`, skip the `run_to_completion(handle).await` call. Additionally, only in Windows, to avoid the child process console being detached to the terminal, the process is spawned with [CREATE_NEW_CONSOLE](https://learn.microsoft.com/en-us/windows/win32/procthread/process-creation-flags). This way it has its own console.

## Test Plan

As this is rough initial attempt, I only tested manually. If the PR is OK to be moved to next phase, I'll add programmatic tests.

1. Create two Python scripts, `a.py` and `b.py`.
```python
a.py

import subprocess
subprocess.check_call(['uv.exe', 'run', 'b.py'])
```

```python
b.py

import time
time.sleep(9999)
```
2. `uv run a.py`.
3. Verify that there is no lingering `uv.exe` process in task manager.

---

_Comment by @zanieb on 2025-10-12 13:05_

There's some relevant discussion on https://github.com/astral-sh/uv/issues/3095

---

_Comment by @CrendKing on 2025-10-12 18:56_

I see. You guys worry about temporary resource cleanup before the child is finished. Sure, there are use case where `uv run` needs to maintain and cleanup like temporary directory between invocations. But there are also other (majority for me) cases where nothing needs to be cleaned. The project is permanent. The dependencies are declared in `pyproject.toml` and saved in `.venv`. If the documentation for this argument explains what can be expected, I think it's fine.

---
