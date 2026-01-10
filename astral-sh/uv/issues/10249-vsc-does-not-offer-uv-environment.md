---
number: 10249
title: VSC does not offer uv environment
type: issue
state: open
author: thistlillo
labels:
  - question
  - external
assignees: []
created_at: 2024-12-31T11:00:18Z
updated_at: 2025-01-04T00:31:57Z
url: https://github.com/astral-sh/uv/issues/10249
synced_at: 2026-01-10T01:24:51Z
---

# VSC does not offer uv environment

---

_Issue opened by @thistlillo on 2024-12-31 11:00_

I have an uv environment In a folder F open inside of VSC. The environment is in the .venv subfolder of F.
When I create a new ipynb file and click on "Select kernel", the list I can choose from contains only conda environment and not the uv environment inside of F.

Is there any way I can select the uv env for running cells inside of the newly created ipynb?

Cheers

Using VSC on Mac OS:

```
Version: 1.95.3
Commit: f1a4fb101478ce6ec82fe9627c43efbf9e98c813
Date: 2024-11-13T14:50:04.152Z
Electron: 32.2.1
ElectronBuildId: 10427718
Chromium: 128.0.6613.186
Node.js: 20.18.0
V8: 12.8.374.38-electron.0
OS: Darwin arm64 24.2.0
```

connected via ssh to a linux machine:

```
Linux hostname 5.15.0-116-generic #126~20.04.1-Ubuntu SMP Mon Jul 1 15:40:07 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

on which I installed:
```
uv 0.4.24
```

Clearly, the uv env works with no issues when used via command line.

---

_Comment by @samypr100 on 2025-01-04 00:31_

Make sure to tell VSC where your .venv is and also try restart your kernel or re-install jupyter from within your .venv. I'd recommend otherwise opening an issue in VSC, as this seems to be an upstream issue with VSC failing to detect virtual environments.

---

_Label `question` added by @samypr100 on 2025-01-04 00:31_

---

_Label `upstream` added by @samypr100 on 2025-01-04 00:31_

---
