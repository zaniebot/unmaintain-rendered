---
number: 13789
title: Why default behavior with no args changed?
type: issue
state: closed
author: KuRRe8
labels:
  - question
assignees: []
created_at: 2025-06-02T19:26:44Z
updated_at: 2025-06-02T21:30:54Z
url: https://github.com/astral-sh/uv/issues/13789
synced_at: 2026-01-10T01:25:38Z
---

# Why default behavior with no args changed?

---

_Issue opened by @KuRRe8 on 2025-06-02 19:26_

### Question

executing `uv` directly will show the help page before, just like this

![Image](https://github.com/user-attachments/assets/6ec2e839-c188-41d7-bd23-4532766941b6)

but today it changed to show an error, what may makes this difference?

![Image](https://github.com/user-attachments/assets/4e9b098d-b7fb-4f68-9873-114a0929b219)

### Platform

Windows10(22H2) + amd64 + Windows Terminal 1.22.11141.0 + powershell 5.1.19041.5848

### Version

v0.7.9

---

_Label `question` added by @KuRRe8 on 2025-06-02 19:26_

---

_Comment by @zanieb on 2025-06-02 19:32_

Huh, I can't reproduce that.

What happens if you do `uvx.exe uv@0.7.8`?

---

_Comment by @KuRRe8 on 2025-06-02 19:34_

> uvx.exe uv@0.7.8

![Image](https://github.com/user-attachments/assets/a8fd6d3d-c8bc-493d-8b3c-7b1c6acf0446)

This change happened less than 1 hour ago, and as I can remember, I have only switched the cache dir of uv, and done nothing else.

---

_Comment by @KuRRe8 on 2025-06-02 19:49_

> Huh, I can't reproduce that.
> 
> What happens if you do `uvx.exe uv@0.7.8`?

confirmed, this is affected by the environment variable UV_CACHE_DIR

---

_Closed by @KuRRe8 on 2025-06-02 20:58_

---

_Comment by @zanieb on 2025-06-02 21:30_

Thanks!

---

_Referenced in [astral-sh/uv#13790](../../astral-sh/uv/issues/13790.md) on 2025-06-02 21:40_

---
