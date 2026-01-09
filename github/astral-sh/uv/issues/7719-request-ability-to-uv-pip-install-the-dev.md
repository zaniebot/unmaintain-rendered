---
number: 7719
title: "Request: ability to `uv pip install` the `dev-dependencies`"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2024-09-26T18:13:38Z
updated_at: 2024-09-26T18:41:40Z
url: https://github.com/astral-sh/uv/issues/7719
synced_at: 2026-01-07T13:12:17-06:00
---

# Request: ability to `uv pip install` the `dev-dependencies`

---

_Issue opened by @jamesbraza on 2024-09-26 18:13_

Multiple of my teammates are not fully integrated with `uv sync`, and just want to install the core package editably with the [`dev-dependencies`](https://docs.astral.sh/uv/reference/settings/#dev-dependencies) from a `pyproject.toml`'s `[uv.tool]`.

Can we have some way for `uv pip install` to install `dev-dependencies`?

My best idea for this is something like `uv pip install -e .[dev]`, if there is no `dev` extra, will fall back on `dev-dependencies`.

Disclaimer: this may be considered in https://github.com/astral-sh/uv/issues/3560 or https://github.com/astral-sh/uv/issues/4730

---

_Comment by @zanieb on 2024-09-26 18:23_

You can use `uv export --only-dev | uv pip sync -`

---

_Label `question` added by @zanieb on 2024-09-26 18:23_

---

_Comment by @jamesbraza on 2024-09-26 18:41_

Okay sounds good, thank you @zanieb ! 👍 

---

_Closed by @jamesbraza on 2024-09-26 18:41_

---
