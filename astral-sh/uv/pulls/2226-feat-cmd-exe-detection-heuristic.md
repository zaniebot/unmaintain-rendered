```yaml
number: 2226
title: "feat: cmd.exe detection heuristic"
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: cmd-detection
created_at: 2024-03-06T02:45:48Z
updated_at: 2024-03-06T03:29:58Z
url: https://github.com/astral-sh/uv/pull/2226
synced_at: 2026-01-10T14:54:43Z
```

# feat: cmd.exe detection heuristic

---

_Pull request opened by @samypr100 on 2024-03-06 02:45_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Follow up from discussion in https://github.com/astral-sh/uv/pull/2223

Detect CMD.exe by checking if `PROMPT` env var is set on windows, otherwise assume it's PowerShell.

Note, this will not work if user modifies their system env vars to include `PROMPT` by default or if they launch nested PowerShell from Command Prompt (e.g. `Developer PowerShell for VS 2022`).

## Test Plan

<!-- How was it tested? -->
Only tested locally, although we try to add some CI tests that specifically use CMD.exe

Command Prompt
```
Microsoft Windows [Version 10.0.19044.3086]
(c) Microsoft Corporation. All rights reserved.

Z:\Users\samypr100\dev\uv>Z:\Users\samypr100\.cargo\bin\cargo.exe +stable run --color=always -- venv "Foo Bar"
    Finished dev [unoptimized + debuginfo] target(s) in 0.69s
     Running `target\debug\uv.exe venv "Foo Bar"`
Using Python 3.12.2 interpreter at: Z:\Users\samypr100\AppData\Local\Programs\Python\Python312\python.exe
Creating virtualenv at: Foo Bar
Activate with: "Foo Bar\Scripts\activate"
```

Power Shell
```
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Try the new cross-platform PowerShell https://aka.ms/pscore6

PS Z:\Users\samypr100\dev\uv>Z:\Users\samypr100\.cargo\bin\cargo.exe +stable run --color=always -- venv "Foo Bar"
    Finished dev [unoptimized + debuginfo] target(s) in 0.63s
     Running `target\debug\uv.exe venv "Foo Bar"`
Using Python 3.12.2 interpreter at: Z:\Users\samypr100\AppData\Local\Programs\Python\Python312\python.exe
Creating virtualenv at: Foo Bar
Activate with: & "Foo Bar\Scripts\activate"
```

---

_Marked ready for review by @samypr100 on 2024-03-06 02:49_

---

_@charliermarsh reviewed on 2024-03-06 03:01_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/venv.rs`:265 on 2024-03-06 03:01_

Maybe we should just merge `shlex_posix` and `shlex_windows`, and pass in `Shell` to toggle the behavior -- wdyt?

---

_@samypr100 reviewed on 2024-03-06 03:04_

---

_Review comment by @samypr100 on `crates/uv/src/commands/venv.rs`:265 on 2024-03-06 03:04_

I kinda like the separation between Posix and Windows, but I'm biased 😆

---

_@charliermarsh reviewed on 2024-03-06 03:16_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/venv.rs`:265 on 2024-03-06 03:16_

Sounds good :)

---

_@charliermarsh approved on 2024-03-06 03:21_

Thanks!

---

_Label `enhancement` added by @charliermarsh on 2024-03-06 03:22_

---

_Label `windows` added by @charliermarsh on 2024-03-06 03:22_

---

_Merged by @charliermarsh on 2024-03-06 03:29_

---

_Closed by @charliermarsh on 2024-03-06 03:29_

---

_Branch deleted on 2024-03-06 03:29_

---
