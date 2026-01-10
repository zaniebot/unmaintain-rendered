---
number: 11749
title: "`uv python install` installing wrong architecture"
type: issue
state: closed
author: Crsk
labels:
  - bug
assignees: []
created_at: 2025-02-24T18:00:19Z
updated_at: 2025-02-24T18:49:10Z
url: https://github.com/astral-sh/uv/issues/11749
synced_at: 2026-01-10T01:25:10Z
---

# `uv python install` installing wrong architecture

---

_Issue opened by @Crsk on 2025-02-24 18:00_

### Summary

I have some error about incompatible architecture ``` ... but is an incompatible architecture (have 'arm64', need 'x86_64') ```

Maybe because my virtual env is on `x86_64` while my architecture is `arm`

Ok so trying to install python from a blank directory:
`> uv python install 3.11.10`
`> uv python list --only-installed`
the new installed version is `x86_64`

why is `uv python install <version>` wrongly installing `x86_64`? Am I doing something wrong?

![Image](https://github.com/user-attachments/assets/8cc716d4-bc41-4b8c-a527-fd5446eed979)

### Platform

macOS 15 arm64

### Version

uv 0.6.2 (Homebrew 2025-02-19)

### Python version

N/A

---

_Label `bug` added by @Crsk on 2025-02-24 18:00_

---

_Comment by @zanieb on 2025-02-24 18:04_

Did you install uv under Rosetta emulation? What's `which uv`? 

---

_Comment by @Crsk on 2025-02-24 18:20_

@zanieb I'm not Rosetta emulating

`> which uv`
`/usr/local/bin/uv`

---

_Comment by @Crsk on 2025-02-24 18:30_

wait, the path `/usr/local/bin` means `x86_64` right? So the issue should come from the `uv` installation... I'll try to reinstall more carefully and see what happens

---

_Comment by @Crsk on 2025-02-24 18:49_

Indeed it was a wrong `brew install`, I tried with `curl -LsSf https://astral.sh/uv/install.sh | sh` and now it works

Thanks @zanieb 

<img width="1130" alt="Image" src="https://github.com/user-attachments/assets/88e2788b-980a-49db-96c7-fcc8040d311c" />

---

_Closed by @Crsk on 2025-02-24 18:49_

---
