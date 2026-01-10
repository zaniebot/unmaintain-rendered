---
number: 6645
title: "Add `UV_DEFAULT_PYTHON` environment variable"
type: issue
state: open
author: unique1o1
labels:
  - configuration
assignees: []
created_at: 2024-08-26T15:19:54Z
updated_at: 2025-02-03T08:12:44Z
url: https://github.com/astral-sh/uv/issues/6645
synced_at: 2026-01-10T01:24:04Z
---

# Add `UV_DEFAULT_PYTHON` environment variable

---

_Issue opened by @unique1o1 on 2024-08-26 15:19_

When adding a `.python-version` file to a project and afterward using `uv sync`, the UV_PYTHON env variable takes more precedence over `.python-version` and doesn't change to the python version specified inside `.python-version`.

My Question:

Is this the intended behaviour?

UV version: 0.3.3 (deea6025a 2024-08-23)

---

_Assigned to @zanieb by @zanieb on 2024-08-26 15:28_

---

_Comment by @zanieb on 2024-08-26 16:34_

I think this is intentional. `UV_PYTHON` is equivalent to setting `--python` and both take precedence over `.python-version` files. If you wanted different behavior, I think we'd need to introduce some sort of `UV_DEFAULT_PYTHON` variable.

---

_Label `question` added by @zanieb on 2024-08-26 16:34_

---

_Comment by @unique1o1 on 2024-08-27 10:30_

IMO, having `UV_DEFAULT_PYTHON`  will be more useful than  `UV_PYTHON`, since we already have `--python`.

---

_Label `configuration` added by @zanieb on 2024-08-27 11:43_

---

_Renamed from "UV_PYTHON env variable takes precedence over .python-version" to "Add `UV_DEFAULT_PYTHON` environment variable" by @zanieb on 2024-08-27 11:43_

---

_Label `question` removed by @zanieb on 2024-08-27 11:43_

---

_Unassigned @zanieb by @zanieb on 2024-08-27 11:44_

---

_Comment by @ksamuel on 2025-02-03 08:12_

I agree. I mistakenly used `UV_PYTHON` to set the default python version when none is passed, and was surprised when running `uv run python` in a project started a different shell version than the one in .python-version.

Having a `UV_DEFAULT_PYTHON` (and signal it exists in the doc  and help message for `UV_PYTHON`) would help avoid this surprise. It's not uncommon to stick to a default version of Python for a while, and update it when needed, so an env var makes for that makes sense.

---

_Referenced in [astral-sh/uv#14748](../../astral-sh/uv/issues/14748.md) on 2025-08-04 21:18_

---
