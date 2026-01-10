```yaml
number: 1904
title: "[BUG] Running `py --list-paths` failed with status exit code: 0"
type: issue
state: closed
author: arghhjayy
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-23T07:40:51Z
updated_at: 2024-02-23T18:40:53Z
url: https://github.com/astral-sh/uv/issues/1904
synced_at: 2026-01-10T05:40:32Z
```

# [BUG] Running `py --list-paths` failed with status exit code: 0

---

_Issue opened by @arghhjayy on 2024-02-23 07:40_

Hello fellow devs,

Thank you for giving us uv, I'm so excited to try it!
I just installed it using pip on my Windows 11: `pip install uv`

I have Anaconda installed, so the shell looks like:
`(base) PS C:\Users\MyUserName>`

and when I try to execute `uv venv` as mentioned in the Getting Started guide in README, I get the following error:

```
  × Running `py --list-paths` failed with status exit code: 0:
  │ --- stdout:
  │ -3.10-64       C:\Users\MyUserName\AppData\Local\Programs\Python\Python310\python.exe
  │  -3.9-64        C:\Users\MyUserName\anaconda3\python.exe
  │ --- stderr:
  │ Installed Pythons found by py Launcher for Windows *
  │ ---
```


---

_Comment by @MichaReiser on 2024-02-23 07:43_

Ah, interesting, so it's Anaconda's py launcher that prints `Pythons found by py launcher for Windows`? 

This should be fixed by https://github.com/astral-sh/uv/pull/1885

---

_Label `bug` added by @MichaReiser on 2024-02-23 07:43_

---

_Label `windows` added by @MichaReiser on 2024-02-23 07:43_

---

_Comment by @arghhjayy on 2024-02-23 07:50_

Thank you for the prompt reply @MichaReiser 

---

_Comment by @konstin on 2024-02-23 15:00_

Do you have instructions how to reproduce this? I'd like to file an upstream bug report

---

_Closed by @zanieb on 2024-02-23 15:05_

---

_Comment by @arghhjayy on 2024-02-23 18:40_

> Do you have instructions how to reproduce this? I'd like to file an upstream bug report

In Windows with Anaconda installed, just install: `pip install uv` and do `uv venv`
@konstin 

---
