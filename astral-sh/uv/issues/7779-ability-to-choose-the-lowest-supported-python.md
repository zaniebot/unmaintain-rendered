---
number: 7779
title: Ability to choose the lowest supported Python version
type: issue
state: closed
author: lengau
labels:
  - duplicate
assignees: []
created_at: 2024-09-29T13:25:50Z
updated_at: 2024-11-08T17:02:32Z
url: https://github.com/astral-sh/uv/issues/7779
synced_at: 2026-01-10T01:24:19Z
---

# Ability to choose the lowest supported Python version

---

_Issue opened by @lengau on 2024-09-29 13:25_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When running `uv sync` it would be nice to be able to choose the lowest Python version supported by the project dynamically (rather than having to specify that version). e.g. `uv sync --python=lowest`


---

_Comment by @malikrohail on 2024-09-29 16:20_

can i fix this?

---

_Comment by @charliermarsh on 2024-09-29 17:26_

As in, the project defines a `requires-python` in the `pyproject.toml`, and you want to use the lowest-supported version rather than the highest?

---

_Comment by @zanieb on 2024-09-29 21:01_

I think we might need a `--python-strategy <lowest | highest | first>` or something. Similar to https://github.com/astral-sh/uv/issues/7562

---

_Comment by @lengau on 2024-09-30 14:45_

@charliermarsh yes that would be the idea :-)

---

_Referenced in [astral-sh/uv#8247](../../astral-sh/uv/issues/8247.md) on 2024-11-08 17:00_

---

_Comment by @zanieb on 2024-11-08 17:02_

Turns out this was previously requested in https://github.com/astral-sh/uv/issues/5609 let's track there

---

_Closed by @zanieb on 2024-11-08 17:02_

---

_Label `duplicate` added by @zanieb on 2024-11-08 17:02_

---

_Referenced in [astral-sh/uv#5609](../../astral-sh/uv/issues/5609.md) on 2024-11-08 17:03_

---
