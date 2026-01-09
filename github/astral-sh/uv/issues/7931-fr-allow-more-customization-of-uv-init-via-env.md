---
number: 7931
title: "FR: allow more customization of uv init via env vars or settings"
type: issue
state: open
author: richieadler
labels:
  - projects
  - configuration
assignees: []
created_at: 2024-10-04T17:18:13Z
updated_at: 2024-10-05T22:21:07Z
url: https://github.com/astral-sh/uv/issues/7931
synced_at: 2026-01-07T13:12:17-06:00
---

# FR: allow more customization of uv init via env vars or settings

---

_Issue opened by @richieadler on 2024-10-04 17:18_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

The current defaults for `uv init` may be inconvenient, so having a way to set them via env var or {pyproject|uv}.toml would be useful.

* Assuming `--vcs git` is irksome for those of us that don't use Git in all cases.
* `--no-readme`, `--no-pin-python` and `--no-workspace` is needed even when one is creating fast test projects where those are not needed.

---

_Comment by @zanieb on 2024-10-04 17:22_

It seems fine to allow configuring these in the `uv.toml`, I think.

---

_Label `projects` added by @zanieb on 2024-10-04 17:22_

---

_Label `configuration` added by @zanieb on 2024-10-04 17:22_

---

_Comment by @chrisrodrigue on 2024-10-05 01:33_

Would configuration in `pyproject.toml` also be possible?

---

_Comment by @chrisrodrigue on 2024-10-05 01:45_

Related: https://github.com/astral-sh/uv/issues/5737

---

_Comment by @zanieb on 2024-10-05 14:49_

Like.. targeting workspace members in that context? It seems like a weird use-case.

---

_Comment by @chrisrodrigue on 2024-10-05 21:55_

More so to enable persistent uv command options and parameters for `uv init`,`uv venv`, `uv sync`, and so forth.

[Hatch](https://hatch.pypa.io/latest/intro/#configuration) and [pdm](https://pdm-project.org/latest/usage/config/#passing-constant-arguments-to-every-pdm-invocation) both support this.

The main benefit for this is so that that uv command settings and customizations are centralized in the project so that all devs working in the codebase can see and use them. This would simplify commands for devs who may always need to use certain options such as `--no-build-isolation` or `--offline`, or in @richieadler’s use case, `--no-readme --no-pin-python --no-workspace`.

Keeping customizations in environment variables or `uv.toml` adds hidden state outside of the project scope and is not the best practice (with the exception of settings you absolutely don’t want in the repository such as credentials or secrets).

---

_Comment by @chrisrodrigue on 2024-10-05 22:20_

I think for flexibility and consistency, a dev could/should have the ability to set every uv “configurable” via: command line, `pyproject.toml`, `uv.toml`, and environment variable, with override resolution performed in that order of priority.

I don’t know what implementation would look like, but I think it could be the same abstract implementation for each `Configurable`, using the command line string as the universal key (same for command line, `pyproject.toml`, `uv.toml`, and env var) which maps to the data/function that actually modifies the command/behavior.

---

_Referenced in [astral-sh/uv#12429](../../astral-sh/uv/issues/12429.md) on 2025-04-01 07:52_

---
