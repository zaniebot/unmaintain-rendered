```yaml
number: 7001
title: "Different venv prompts in `uv venv` and `uv sync`"
type: issue
state: closed
author: gusutabopb
labels:
  - needs-decision
assignees: []
created_at: 2024-09-04T10:01:34Z
updated_at: 2024-09-04T15:16:32Z
url: https://github.com/astral-sh/uv/issues/7001
synced_at: 2026-01-10T04:45:10Z
```

# Different venv prompts in `uv venv` and `uv sync`

---

_Issue opened by @gusutabopb on 2024-09-04 10:01_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

As per https://github.com/astral-sh/uv/issues/1445#issuecomment-1953050060, `uv venv` sets the venv prompt to the current directory name (`basename $PWD`):

```bash
> basename $PWD
uv_test
> uv venv -q  && grep prompt .venv/pyvenv.cfg
prompt = uv_test
```

However, when using `uv sync`, the prompt is not set and defaults to `.venv` (the venv directory name)
```bash
> basename $PWD
uv_test
> uv init -q && uv sync -q && grep prompt .venv/pyvenv.cfg
# error code 1
```

I understand `uv` tries to abstract venvs away from the user and that activating them shouldn't really be needed when using `uv run`, etc. However, many people still activate virtual environments and having different prompts depending on which `uv` command created the venv feels inconsistent.

I'd prefer `uv sync` to behave like `uv venv` and set the prompt to the current directory name.

### uv info
```
uv 0.4.4 (3d75df6ab 2024-09-04)
```

---

_Label `needs-decision` added by @charliermarsh on 2024-09-04 13:16_

---

_Comment by @zanieb on 2024-09-04 13:49_

I think we should use the project name instead of `.venv`.

---

_Closed by @zanieb on 2024-09-04 15:16_

---

_Closed by @zanieb on 2024-09-04 15:16_

---
