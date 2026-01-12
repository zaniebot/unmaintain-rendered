```yaml
number: 12738
title: "Why is not `pip` installed inside `venv` by default (`uv venv --seed`)?"
type: issue
state: closed
author: Danipulok
labels:
  - question
assignees: []
created_at: 2025-04-08T08:46:42Z
updated_at: 2025-04-08T17:25:38Z
url: https://github.com/astral-sh/uv/issues/12738
synced_at: 2026-01-12T16:01:11Z
```

# Why is not `pip` installed inside `venv` by default (`uv venv --seed`)?

---

_@Danipulok_

### Question

Hey all!
At first, I thought you don't allow `pip` to be installed at all on a `venv` creation.
But now that I've found `--seed`, the question I have is why is it not enabled by default?

I use Windows 10, and I always check if I'm inside `venv` via `pip list` and `where pip`.
Also, I sometime use commands like `pip list` and `pip show <lib>`.
And I was very confused, when I realized everything's alright, and you just don't add `pip` inside `venv`.

What's the reasoning for not installing `pip` by default?

### Platform

MINGW64_NT-10.0-19045 3.4.10-87d57229.x86_64 x86_64 Msys

### Version

uv 0.6.13 (a0f5c7250 2025-04-07)

---

_Label `question` added by @Danipulok on 2025-04-08 08:46_

---

_Renamed from "Why is there no `pip` executable inside `venv`?" to "Why is not `pip` installed inside `venv` by default (`uv venv --seed`)?" by @Danipulok on 2025-04-08 08:51_

---

_Comment by @FishAlchemist on 2025-04-08 12:40_

Related:
* https://github.com/astral-sh/uv/issues/1330
* https://github.com/astral-sh/uv/issues/3876

---

_Closed by @zanieb on 2025-04-08 17:25_

---

_Comment by @zanieb on 2025-04-08 17:25_

See also https://github.com/astral-sh/uv/issues/12604

---
