---
number: 15802
title: uv run command exec in nodejs child process will trigger local dependency installation
type: issue
state: closed
author: zshnb
labels:
  - question
assignees: []
created_at: 2025-09-12T04:05:29Z
updated_at: 2025-09-13T07:58:47Z
url: https://github.com/astral-sh/uv/issues/15802
synced_at: 2026-01-10T01:26:00Z
---

# uv run command exec in nodejs child process will trigger local dependency installation

---

_Issue opened by @zshnb on 2025-09-12 04:05_

### Summary

I have a nodejs project, contains toolchain folder, toolchain is a python project managed by uv, the folder structure is below
./
./toolchain
./toolchain/lib/faster-whisper

toolchain has a pyproject.toml
```
[project]
name = "asr-service-v2"
version = "0.1.0"
description = "Asr service v2"
requires-python = ">=3.11"
dependencies = [
    "faster-whisper",
    "flask==3.1.2",
    "pytest==8.4.1",
    "torch==2.8.0",
]

[tool.uv.workspace]
members = ["lib/faster_whisper"]

[tool.uv.sources]
faster-whisper = { workspace = true }
```

I've installed dependency by uv sync --frozen, then I exec python script in nodejs, like
```
spawn(uv, [run --project=toolchain python transcriber.py])
```

after I run nodejs, I saw log below
Building faster-whisper @ file:///usr/src/app/toolchain/lib/faster_whisper"
Built faster-whisper @ file:///usr/src/app/toolchain/lib/faster_whisper
Uninstalled 1 package in 0.45ms
Installed 1 package in 0.44ms

how can I fix this problem?

### Platform

ubuntu22.04

### Version

0.8.4 (e176e1714 2025-07-30)

### Python version

python 3.11.5

---

_Label `bug` added by @zshnb on 2025-09-12 04:05_

---

_Comment by @konstin on 2025-09-12 06:28_

Can you check with `uv run -v` or `uv run -vv`? This should tell you the reason for the rebuild.

---

_Label `bug` removed by @konstin on 2025-09-12 06:29_

---

_Label `question` added by @konstin on 2025-09-12 06:29_

---

_Closed by @zshnb on 2025-09-13 07:58_

---
