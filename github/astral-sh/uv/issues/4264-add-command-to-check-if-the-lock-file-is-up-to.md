---
number: 4264
title: Add command to check if the lock file is up to date with the project metadata
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-06-12T03:28:38Z
updated_at: 2025-12-06T00:17:51Z
url: https://github.com/astral-sh/uv/issues/4264
synced_at: 2026-01-07T13:12:17-06:00
---

# Add command to check if the lock file is up to date with the project metadata

---

_Issue opened by @zanieb on 2024-06-12 03:28_

e.g. as in `poetry check --lock` (not yet sure why they deprecated `poetry lock --check`)

---

_Label `cli` added by @zanieb on 2024-06-12 03:28_

---

_Label `preview` added by @zanieb on 2024-06-12 03:28_

---

_Comment by @zanieb on 2024-06-12 14:30_

Some discussion about the `poetry lock --check` change at https://github.com/python-poetry/poetry/pull/8015 and https://github.com/python-poetry/poetry/issues/6756

We ought to have a top-level `uv check` command eventually too but should it do this? I guess `uv check --locked` is an option?

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Comment by @konstin on 2024-09-17 08:58_

It's a bit silly, but `uv lock --locked` does that.

---

_Comment by @asanglard on 2024-09-17 11:56_

> It's a bit silly, but `uv lock --locked` does that.

I didn't saw initially that `uv lock` does not upgrade the dependencies by default.

So, this usage of the command is a good solution to check that there is no "breaking" changes in the dep (missing dep for example...) and it does not try to upgrade the existing dep.

---

_Comment by @charliermarsh on 2024-09-17 12:34_

Yeah the locked flag is the intended solution here.

---

_Referenced in [CSSUoB/TeX-Bot-Py-V2#329](../../CSSUoB/TeX-Bot-Py-V2/issues/329.md) on 2024-09-20 22:48_

---

_Closed by @charliermarsh on 2024-11-20 00:31_

---

_Comment by @hbelmiro on 2025-12-06 00:17_

I wrote a GitHub action for that. It also validades requirements.txt, if needed. 

https://github.com/hbelmiro/uv-lock-check

---
