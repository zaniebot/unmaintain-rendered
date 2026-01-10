---
number: 6833
title: src directory is not used for --app and default Projects
type: issue
state: closed
author: denkasyanov
labels:
  - documentation
assignees: []
created_at: 2024-08-29T22:12:47Z
updated_at: 2024-08-30T18:33:21Z
url: https://github.com/astral-sh/uv/issues/6833
synced_at: 2026-01-10T01:24:06Z
---

# src directory is not used for --app and default Projects

---

_Issue opened by @denkasyanov on 2024-08-29 22:12_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv 0.4.0 (d9bd3bc7a 2024-08-28)

After one of the previous releases (I believe after 0.4.0) FastAPI docs on initailizing a project looks a little bit ouf of date https://docs.astral.sh/uv/guides/integration/fastapi/#initializing-a-fastapi-project

After some experimenting I noticed that initializing a project with one of the following two commands doesn't create `src` directory with `uv 0.4.0`.

`uv init app` (default)
`uv init --app app` (App project)

Currently FastAPI integration docs linked above say

> uv uses a `src` layout for newly-created projects 

But it only seems to be true for commands like `uv init --lib app`  which doesn't look applicable.

Accordingly, the tree structure example
/https://github.com/astral-sh/uv/blame/main/docs/guides/integration/fastapi.md#L100-L115

and the command
https://github.com/astral-sh/uv/blame/main/docs/guides/integration/fastapi.md#L119-L121

seem to also need to be adjusted for the current `uv init` behaviour

Initially I tried to follow the FastAPI integration guide exclusively. When I couldn't make it work as described I tried to reference other parts of docs (`uv init` reference and Projects section). Although I didn't find any incositencies, it felt like the behavior around `src` directory could be documented a little bit more explicitly (for us `uv` newcomers), especially after adding `--app` and `--lib`.

I would like to create a PR for FastAPI integration docs, and optionally for clarifying directory structure behaviour. 

---

_Renamed from "src directory is not default for --app and default Projects" to "src directory is not used for --app and default Projects" by @denkasyanov on 2024-08-29 22:18_

---

_Comment by @zanieb on 2024-08-29 22:18_

I'd definitely appreciate some fixes to the FastAPI docs — I gave them a quick pass in #6752 but it sounds like I missed some stuff.

---

_Label `documentation` added by @zanieb on 2024-08-29 22:19_

---

_Referenced in [astral-sh/uv#6850](../../astral-sh/uv/pulls/6850.md) on 2024-08-30 02:56_

---

_Closed by @denkasyanov on 2024-08-30 18:33_

---
