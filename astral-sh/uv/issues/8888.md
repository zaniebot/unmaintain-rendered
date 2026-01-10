```yaml
number: 8888
title: "Can't run some scripts on Windows with `uv tool run`"
type: issue
state: open
author: depau
labels:
  - bug
  - windows
assignees: []
created_at: 2024-11-07T14:41:00Z
updated_at: 2024-11-11T14:51:35Z
url: https://github.com/astral-sh/uv/issues/8888
synced_at: 2026-01-10T04:36:20Z
```

# Can't run some scripts on Windows with `uv tool run`

---

_Issue opened by @depau on 2024-11-07 14:41_

uv version: `uv 0.4.30 (61ed2a236 2024-11-04)`

[`awslocal`](https://github.com/localstack/awscli-local?tab=readme-ov-file#Limitations) on Windows doesn't seem to work properly with `uv tool run --from awscli-local awslocal`

```
The executable `awslocal` was not found.
warning: An executable named `awslocal` is not provided by package `awscli-local`.
The following executables are provided by `awscli-local`:
- awslocal
- awslocal.bat
Consider using `uv tool run --from awscli-local <EXECUTABLE_NAME>` instead.
```

The `awslocal` file is actually a script:

```python
#!python.exe

"""
Thin wrapper around the "aws" command line interface (CLI) for use
with LocalStack.
...
```

while for other tools it looks like a `.exe` is installed, which seems to work with `uv`.

This is probably caused by the fact that they provide the entry scripts as a path to file, not as `module:function`.

Using the provided `.bat` file does work, but it requires providing a different executable name based on the current OS and it looks like python detaches from the terminal when run from a batch script.

I'm not sure whether this is a problem with awslocal or uv. I thought I'd report it here first since this usage of setuptools seems legit.

---

_Comment by @charliermarsh on 2024-11-09 02:16_

Thanks! Makes sense to report it here. We'll take a look.

---

_Label `bug` added by @charliermarsh on 2024-11-09 02:16_

---

_Label `windows` added by @charliermarsh on 2024-11-09 02:16_

---

_Comment by @samypr100 on 2024-11-09 17:31_

I think this is expected/known to fail due to their use of legacy `scripts` rather than `console_scripts`.

Related:
* https://github.com/astral-sh/uv/issues/6661
* https://github.com/astral-sh/uv/issues/8770
  * https://github.com/astral-sh/uv/issues/8770#issuecomment-2453508632

---

_Comment by @zanieb on 2024-11-11 14:51_

I think we should probably special case them if it's feasible.

---
