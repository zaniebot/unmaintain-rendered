```yaml
number: 10037
title: "Add `pip` alias for `uv pip` to virtual environments"
type: issue
state: closed
author: mosmuell
labels:
  - duplicate
assignees: []
created_at: 2024-12-19T17:41:25Z
updated_at: 2025-01-25T16:22:11Z
url: https://github.com/astral-sh/uv/issues/10037
synced_at: 2026-01-12T16:00:05Z
```

# Add `pip` alias for `uv pip` to virtual environments

---

_@mosmuell_

Hello,

In my typical workflow, I create virtual environments using `poetry` or `venv` and activate them with `source .venv/bin/activate`. While I primarily rely on `poetry` for package management and tracking dependencies in `pyproject.toml`, I occasionally use `pip` for tasks like checking installed package versions or installing packages without tracking them in `pyproject.toml`.

Currently, this workflow isn’t possible with `uv` because virtual environments created by `uv` do not include `pip` by default. To address this, would it be possible to create an alias for `pip` that maps to `uv pip` within UV-managed virtual environments? This would ensure a seamless experience when using `pip` commands alongside `uv`.

Thank you for considering this feature request!

---

_Comment by @FishAlchemist on 2024-12-19 17:59_

This is a rather old question (https://github.com/astral-sh/uv/issues/1657). Perhaps you could take a look at the discussion to see why uv didn't plan to do this in the past.

---

_Label `duplicate` added by @zanieb on 2024-12-19 18:13_

---

_Closed by @zanieb on 2024-12-19 18:13_

---

_Comment by @mosmuell on 2024-12-19 19:45_

Thanks for the fast reply! I did not find the linked issue by myself.

---

_Renamed from "Add `pip` Alias in UV-Managed Virtual Environments" to "Add `pip` alias for `uv pip` to virtual environments" by @zanieb on 2025-01-25 16:22_

---
