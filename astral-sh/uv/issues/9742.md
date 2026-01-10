```yaml
number: 9742
title: uvx not working on windows
type: issue
state: closed
author: pythonweb2
labels:
  - question
  - windows
assignees: []
created_at: 2024-12-09T16:55:35Z
updated_at: 2024-12-10T15:40:55Z
url: https://github.com/astral-sh/uv/issues/9742
synced_at: 2026-01-10T04:36:21Z
```

# uvx not working on windows

---

_Issue opened by @pythonweb2 on 2024-12-09 16:55_

In the past two versions (0.5.6 and 0.5.7), I have been getting this error message when trying to use uvx:

```
PS> uvx --help
error: The system cannot find the file specified. (os error 2)
```

So I would assume this is some sort of bug, since it was working in the past for me.

Thanks!

---

_Comment by @charliermarsh on 2024-12-09 18:16_

Hmm. My guess is that the install is in a weird state. We assume that `uv.exe` is next to `uvx.exe`, and that's the message I'd expect if `uv.exe` is no longer there. What does `where uvx` show? Is there a `uv.exe` next to it?

---

_Label `question` added by @charliermarsh on 2024-12-09 18:16_

---

_Label `windows` added by @charliermarsh on 2024-12-09 18:16_

---

_Comment by @pythonweb2 on 2024-12-10 15:40_

Yeah I'm not quite sure how it got into this state, but this is what was happening:

```
PS> Get-Command uvx

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Application     uvx.exe                                            0.0.0.0    ~\.cargo\uvx.exe

PS> Get-Command uv

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Application     uv.exe                                             0.0.0.0    ~\.cargo\bin\uv.exe
```

I deleted `~\.cargo\uvx.exe` and now it works. Thanks!

---

_Closed by @pythonweb2 on 2024-12-10 15:40_

---
