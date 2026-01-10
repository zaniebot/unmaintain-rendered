---
number: 9257
title: UV workspace docker build?
type: issue
state: closed
author: ejiang-eog
labels: []
assignees: []
created_at: 2024-11-19T22:45:59Z
updated_at: 2025-04-01T20:39:27Z
url: https://github.com/astral-sh/uv/issues/9257
synced_at: 2026-01-10T01:24:38Z
---

# UV workspace docker build?

---

_Issue opened by @ejiang-eog on 2024-11-19 22:45_

# I want to build a docker image only for a project in the workspace, but it is saying error. Where should i run the dockerfile and how should i build it?
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @Job-D2X on 2025-01-16 14:04_

I'm running into the same problem. I have some local dependencies in my workspace, but building an imange for my entire workspace feels wrong


---

_Comment by @stefanadelbert on 2025-03-31 14:50_

I'm having issues with a similar scenario. See #11398.

---

_Comment by @zanieb on 2025-04-01 20:39_

I think you want `uv sync --package` for this generally? I'll follow up on #11398 

I'm going to close this because there aren't nearly enough details for me to help. Feel free to respond with minimal examples and I can help more.

---

_Closed by @zanieb on 2025-04-01 20:39_

---
