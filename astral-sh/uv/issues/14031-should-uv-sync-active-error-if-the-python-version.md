---
number: 14031
title: "Should `uv sync --active` error if the Python version request differs from the environment?"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2025-06-13T16:32:57Z
updated_at: 2025-06-14T09:11:58Z
url: https://github.com/astral-sh/uv/issues/14031
synced_at: 2026-01-10T01:25:41Z
---

# Should `uv sync --active` error if the Python version request differs from the environment?

---

_Issue opened by @zanieb on 2025-06-13 16:32_

Right now, we'll invalidate and recreate the environment (https://github.com/astral-sh/uv/issues/13830) but for `--active` in particular, we may want to just error?

---

_Label `needs-decision` added by @zanieb on 2025-06-13 16:32_

---

_Comment by @zanieb on 2025-06-13 16:33_

For an implicit request, e.g., a `.python-version` file, should we ignore the request and just use the environment?

---

_Comment by @DRKolev-code on 2025-06-14 09:11_

Should there not be a warning if your `.venv` and `.python-version` disagree with a question if it should
1) sync `.venv` to `.python_version`
2) sync `.python-version` to `.venv`
3) ignore

Even for `uv lock`.

---
