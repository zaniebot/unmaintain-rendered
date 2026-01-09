---
number: 7342
title: "[Feature request] Avoiding .venv in production"
type: issue
state: closed
author: pkucmus
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-09-12T20:20:08Z
updated_at: 2024-09-13T08:10:36Z
url: https://github.com/astral-sh/uv/issues/7342
synced_at: 2026-01-07T13:12:17-06:00
---

# [Feature request] Avoiding .venv in production

---

_Issue opened by @pkucmus on 2024-09-12 20:20_

Related to: https://github.com/astral-sh/uv/issues/7341

When making a Docker image with my application I would like to achieve the equivalent of Poetry's `POETRY_VIRTUALENVS_CREATE=false` to use the only Python interpreter in the container. This would reduce need for consideration over the .venv directory being mixed in the host and in the container (as the container would not have one)

---

_Comment by @jond01 on 2024-09-12 20:27_

Don't the following satisfy your request:
* `--system` flag:
  https://docs.astral.sh/uv/reference/settings/#pip_system
* `UV_SYSTEM_PYTHON=true` environment variable:
  https://docs.astral.sh/uv/configuration/environment/

?

---

_Referenced in [astral-sh/uv#7343](../../astral-sh/uv/issues/7343.md) on 2024-09-12 20:28_

---

_Comment by @zanieb on 2024-09-12 20:33_

Please see https://docs.astral.sh/uv/concepts/projects/#configuring-the-project-environment-path and https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment

---

_Comment by @zanieb on 2024-09-12 20:37_

This was addressed in https://github.com/astral-sh/uv/pull/6834

---

_Closed by @zanieb on 2024-09-12 20:37_

---

_Label `duplicate` added by @zanieb on 2024-09-12 20:37_

---

_Label `question` added by @zanieb on 2024-09-12 20:37_

---

_Comment by @pkucmus on 2024-09-12 20:51_

Sorry, I saw this but did not connect the dots that it's about that problem, for anyone else, https://github.com/astral-sh/uv/pull/6834#issuecomment-2319253359 this is a perfect solution.

---

_Comment by @zanieb on 2024-09-13 01:44_

No problem we can work on improving it. Is there anything in the docs that would be helpful to change?

---

_Comment by @pkucmus on 2024-09-13 08:07_

> Don't the following satisfy your request:
> 
>     * `--system` flag:
>       [docs.astral.sh/uv/reference/settings#pip_system](https://docs.astral.sh/uv/reference/settings/#pip_system)
> 
>     * `UV_SYSTEM_PYTHON=true` environment variable:
>       [docs.astral.sh/uv/configuration/environment](https://docs.astral.sh/uv/configuration/environment/)
> 
> 
> ?

Don't see the `--system` flag in 0.4.9 but I do see --python-preference=only-system
but then when I set `ENV UV_PYTHON_PREFERENCE=only-system` `uv sync --frozen --no-install-project --no-dev` still creates `.venv`
`uv --system sync` errors out `error: unexpected argument '--system' found`

---

_Comment by @pkucmus on 2024-09-13 08:10_

@zanieb I'm currently looking into moving from pyenv, pipx, Poetry to uv and set uv as a company standard, I will explore more and can share my journey and/or feedback here, if that "Poetry user's journey" point of view would be at all helpful to you and your team working on uv.

---
