---
number: 7253
title: Allow .uv.toml
type: issue
state: open
author: AntoineD
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-09-10T13:08:52Z
updated_at: 2024-09-10T21:39:31Z
url: https://github.com/astral-sh/uv/issues/7253
synced_at: 2026-01-10T01:24:12Z
---

# Allow .uv.toml

---

_Issue opened by @AntoineD on 2024-09-10 13:08_

Like for most of the tools out there, ruff included, would it be possible to allow the configuration file to be prefixed with `.`: `.uv.toml`?


---

_Comment by @nikhilweee on 2024-09-10 14:57_

`uv` already supports `uv.toml`. Besides, you can also specify options in the `[tool.uv.*]` sections in `pyproject.toml`
Relevant docs here: https://docs.astral.sh/uv/configuration/files/

---

_Comment by @AntoineD on 2024-09-10 16:02_

> `uv` already supports `uv.toml`. Besides, you can also specify options in the `[tool.uv.*]` sections in `pyproject.toml` Relevant docs here: https://docs.astral.sh/uv/configuration/files/

I know `uv` supports `uv.toml`, like `ruff` supports `ruff.toml`: I'd also like `uv` to support `.uv.toml` like `ruff` supports `.ruff.toml`.

---

_Label `configuration` added by @charliermarsh on 2024-09-10 21:39_

---

_Label `needs-decision` added by @charliermarsh on 2024-09-10 21:39_

---
