---
number: 10262
title: can not install torch 2.5.1 on macos with python 3.13.1
type: issue
state: closed
author: whisper-bye
labels: []
assignees: []
created_at: 2025-01-01T13:24:01Z
updated_at: 2025-01-05T16:48:39Z
url: https://github.com/astral-sh/uv/issues/10262
synced_at: 2026-01-07T13:12:18-06:00
---

# can not install torch 2.5.1 on macos with python 3.13.1

---

_Issue opened by @whisper-bye on 2025-01-01 13:24_

```shell
uv init demo -p 3.13.1

cd demo

uv add torch
Using CPython 3.13.1
Creating virtual environment at: .venv
Resolved 23 packages in 7ms
error: Distribution `torch==2.5.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

---

_Comment by @whisper-bye on 2025-01-01 13:24_

macOS 15.0

---

_Comment by @YouJiacheng on 2025-01-01 13:39_

![Image](https://github.com/user-attachments/assets/d9f80891-54a3-4174-accd-59dbc6b6e9c0)
PyTorch currently doesn't have a cp313 wheel for macOS.

---

_Comment by @whisper-bye on 2025-01-01 13:43_

@YouJiacheng thanks for your quick reply.

---

_Closed by @whisper-bye on 2025-01-01 13:43_

---

_Comment by @dsantiago on 2025-01-05 05:47_

> ![Image](https://github.com/user-attachments/assets/d9f80891-54a3-4174-accd-59dbc6b6e9c0) PyTorch currently doesn't have a cp313 wheel for macOS.

Curious the wheel file for mac=63mb, windows=203mb and linux=906mb...

Why so much bigger? 🤷🏻‍♂️

---

_Comment by @YouJiacheng on 2025-01-05 05:53_

Linux's default is with cuda, macOS is cpu only.
Forget what's default of Windows.

---

_Comment by @dsantiago on 2025-01-05 16:48_

Thanks for clarification even tough it was an off topic question...

---
