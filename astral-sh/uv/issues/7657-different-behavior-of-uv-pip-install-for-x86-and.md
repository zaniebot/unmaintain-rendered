---
number: 7657
title: different behavior of uv pip install for x86 and aarch64 archs
type: issue
state: closed
author: senysenyseny16
labels:
  - external
assignees: []
created_at: 2024-09-24T12:12:44Z
updated_at: 2024-09-24T12:22:26Z
url: https://github.com/astral-sh/uv/issues/7657
synced_at: 2026-01-10T01:24:17Z
---

# different behavior of uv pip install for x86 and aarch64 archs

---

_Issue opened by @senysenyseny16 on 2024-09-24 12:12_

Hello, I encountered the following problem:

```
x86     | pip install torch==2.1.0+cpu --extra-index-url https://download.pytorch.org/whl/cpu       | ✅
x86     | pip install torch==2.1.0 --extra-index-url https://download.pytorch.org/whl/cpu           | ✅ --+
x86     | uv pip install torch==2.1.0+cpu --extra-index-url https://download.pytorch.org/whl/cpu    | ✅   |
x86     | uv pip install torch==2.1.0 --extra-index-url https://download.pytorch.org/whl/cpu        | ❌   |
                                                                                                     [intersection]
aarch64 | pip install torch==2.1.0+cpu --extra-index-url https://download.pytorch.org/whl/cpu       | ❌   |
aarch64 | pip install torch==2.1.0 --extra-index-url https://download.pytorch.org/whl/cpu           | ✅ --+
aarch64 | uv pip install torch==2.1.0+cpu --extra-index-url https://download.pytorch.org/whl/cpu    | ❌
aarch64 | uv pip install torch==2.1.0 --extra-index-url https://download.pytorch.org/whl/cpu        | ✅
```

With `pip`, you can use `pip install torch==2.1.0 --extra-index-url https://download.pytorch.org/whl/cpu` command and it will work on both architectures, but for `uv`, you'll have to add a condition based on the architecture, which is very inconvenient, especially in the case of automation.

---

_Label `upstream` added by @charliermarsh on 2024-09-24 12:17_

---

_Comment by @charliermarsh on 2024-09-24 12:17_

It's because the PyTorch index itself is inconsistent. Some of the wheels on that index use `+cpu`, and others don't, but they're all CPU wheels. I don't see this as something we can solve without redoing local variant supports and I don't anticipate doing that in the near future.

![Screenshot 2024-09-23 at 3 26 14 PM](https://github.com/user-attachments/assets/82385fa5-3960-4c12-85a5-2682e6d525fa)


---

_Comment by @senysenyseny16 on 2024-09-24 12:21_

ok, thanks for the answer, I think I need to ask the Torch team why they did it that way.

---

_Closed by @senysenyseny16 on 2024-09-24 12:21_

---

_Comment by @charliermarsh on 2024-09-24 12:22_

I'm also interested, I really want to improve the Torch experience. I've seen https://github.com/pytorch/pytorch/issues/136275.

---
