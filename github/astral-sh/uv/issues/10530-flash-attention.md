---
number: 10530
title: "Flash Attention!"
type: issue
state: closed
author: dsantiago
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-01-12T00:44:13Z
updated_at: 2025-01-12T16:35:59Z
url: https://github.com/astral-sh/uv/issues/10530
synced_at: 2026-01-07T13:12:18-06:00
---

# Flash Attention!

---

_Issue opened by @dsantiago on 2025-01-12 00:44_

I tried the Build Isolation documentation:

https://docs.astral.sh/uv/concepts/projects/config/#build-isolation

And many other attempts from the issues around here...

And I still can't create an empty project that works with **flash-attn-v2**...

Probably I should fix many of the version to it to work also but sadly I can't find the any correct option.

---

_Comment by @zanieb on 2025-01-12 01:19_

Please provide a concrete example of your attempt and the error.

See #9452 for guidance.

---

_Label `question` added by @zanieb on 2025-01-12 01:19_

---

_Label `needs-mre` added by @zanieb on 2025-01-12 01:19_

---

_Comment by @dsantiago on 2025-01-12 01:49_

Nevermind the error was on me... CUDA was on 12.4 but nvcc was on 11.4.

With the right compiler version it works (should be >=11.7):

`nvcc -V`
<img width="400" alt="Image" src="https://github.com/user-attachments/assets/f382793e-1089-474d-b9b3-7040169c6bb5" />

With a fresh project do:

```bash
uv pip install torch
pip install psutil
pip install flash-attn --no-build-isolation
```

---

_Closed by @dsantiago on 2025-01-12 01:49_

---

_Comment by @zanieb on 2025-01-12 06:06_

Glad you got it working, thanks for following up!

---

_Comment by @dsantiago on 2025-01-12 16:35_

> Glad you got it working, thanks for following up!

I that appreciate the effort and passion by the team sharing this wonderful tool. Even responding at Saturday...

---
