---
number: 7489
title: Warning using pre-commit and uv
type: issue
state: closed
author: Kludex
labels:
  - error messages
assignees: []
created_at: 2024-09-18T09:54:26Z
updated_at: 2024-09-18T15:34:28Z
url: https://github.com/astral-sh/uv/issues/7489
synced_at: 2026-01-10T01:24:15Z
---

# Warning using pre-commit and uv

---

_Issue opened by @Kludex on 2024-09-18 09:54_

I'm running ruff on the pre-commit job on the GitHub CI.

```
      - id: ruff
        name: Ruff
        entry: uv run ruff
        args: ["check", "--fix"]
        types: [python]
        language: system
      - id: ruff-format
        name: Ruff Format
        entry: uv run ruff
        args: [format]
        language: system
        types: [python]
```

But it causes this:

<img width="986" alt="Screenshot 2024-09-18 at 11 52 53" src="https://github.com/user-attachments/assets/6e59b941-6c1d-45d4-b776-97cbaca97c51">

Using `uvx` doesn't solve the issue, it just changes the message to get the tools cache.

How can I not see the warning?

---

_Comment by @charliermarsh on 2024-09-18 14:22_

I don't think this message should be user-facing \cc @zanieb 

---

_Label `error messages` added by @charliermarsh on 2024-09-18 14:24_

---

_Comment by @Kludex on 2024-09-18 14:33_

We do use the `--verbose` flag on `pre-commit`. FYI 

---

_Comment by @zanieb on 2024-09-18 14:40_

Yeah idk why this is user-facing, maybe because it could take a while?

---

_Referenced in [astral-sh/uv#7502](../../astral-sh/uv/pulls/7502.md) on 2024-09-18 14:41_

---

_Closed by @zanieb on 2024-09-18 15:34_

---

_Referenced in [astral-sh/uv#16112](../../astral-sh/uv/issues/16112.md) on 2025-10-06 08:35_

---

_Referenced in [astral-sh/uv#16138](../../astral-sh/uv/pulls/16138.md) on 2025-10-06 16:42_

---
