---
number: 8761
title: uv does not pass PID 1 to child processes
type: issue
state: closed
author: ejungcl
labels:
  - duplicate
assignees: []
created_at: 2024-11-01T21:36:52Z
updated_at: 2024-11-01T23:49:23Z
url: https://github.com/astral-sh/uv/issues/8761
synced_at: 2026-01-07T13:12:18-06:00
---

# uv does not pass PID 1 to child processes

---

_Issue opened by @ejungcl on 2024-11-01 21:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv version: uv 0.4.23 (83f835b0d 2024-10-17)

When using uv to run a containerized gunicorn app, it will not pass along SIGTERM to children.

If I install a flask app with pip and run it with gunicorn as the ENTRYPOINT or CMD, it will handle SIGTERM as expected. However, if I use 'uv run', it will hang.

The only difference between the two run command is this:
CMD ["gunicorn", "--config", ...]
vs.
CMD["uv", "run", "gunicorn", "--config", ...]

http://blog.dscpl.com.au/2015/12/issues-with-running-as-pid-1-in-docker.html
https://petermalmgren.com/signal-handling-docker/


---

_Comment by @zanieb on 2024-11-01 23:49_

This is a duplicate of https://github.com/astral-sh/uv/issues/6724

---

_Closed by @zanieb on 2024-11-01 23:49_

---

_Label `duplicate` added by @zanieb on 2024-11-01 23:49_

---
