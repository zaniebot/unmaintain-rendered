---
number: 9297
title: sync --all-groups not compatible with --no-dev
type: issue
state: closed
author: hcoohb
labels:
  - bug
assignees: []
created_at: 2024-11-20T23:52:09Z
updated_at: 2024-11-21T01:06:07Z
url: https://github.com/astral-sh/uv/issues/9297
synced_at: 2026-01-07T13:12:18-06:00
---

# sync --all-groups not compatible with --no-dev

---

_Issue opened by @hcoohb on 2024-11-20 23:52_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi,
I am using uv `0.5.4`.
First thanks a lot for introducing the `--all-groups` option in the sync command, very useful.
However, it seems that if I combine it with `--no-dev` the dev dependency group still get installed:
```
uv sync --all-groups --no-dev
```
Results in all groups *Including* dev to be installed.


Quite surprisingly,
```
uv sync --all-groups --no-group dev
```
does install everything but the dev group as expected

---

_Comment by @charliermarsh on 2024-11-21 00:06_

Thanks! That just sounds like a bug.

---

_Label `bug` added by @charliermarsh on 2024-11-21 00:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-21 00:10_

---

_Referenced in [astral-sh/uv#9300](../../astral-sh/uv/pulls/9300.md) on 2024-11-21 00:40_

---

_Closed by @charliermarsh on 2024-11-21 01:06_

---
