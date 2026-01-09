---
number: 2383
title: pip incompatibility
type: issue
state: closed
author: AntoineD
labels:
  - duplicate
assignees: []
created_at: 2024-03-12T14:43:27Z
updated_at: 2024-04-01T21:32:02Z
url: https://github.com/astral-sh/uv/issues/2383
synced_at: 2026-01-07T13:12:17-06:00
---

# pip incompatibility

---

_Issue opened by @AntoineD on 2024-03-12 14:43_

Hello,

via `tox` with `tox-uv`, I got different environment packges as compared to `tox` without `tox-uv`.

This can be reproduced out of `tox` and summarized to the following situation:
- package `A` depends on `B`
- the last release of `A` on PyPI is `2.X.Y` which depends on `B==1`
- the development version of `A` in git depends on `B==2`
- `requirements.txt` (generated with `uv pip compile`) contains:
  - `A @ git+https://...`
  - `B==2`
- in a fresh environment
- first, `uv pip install -r requirements.txt` installs:
  - `A==2.X.Y.dev219+g852ddbeb`
  - `B==2`
- then, `uv pip install 'A>=2'` installs
  - `A==2.X.Y`
  - `B==1`
- with `pip`, the last command does not update `A` and `B` in the environment.

Is this intended?
If yes, is there a workaround to get the same behavior as `pip`?

---

_Comment by @charliermarsh on 2024-03-12 14:45_

I think this is the same as https://github.com/astral-sh/uv/issues/1661. We don't respect the virtual environment as a package source, so when you do the second install, we don't find `A==2.X.Y.dev219+g852ddbeb` since it's not provided via any input source. You'd need to include the `A @ git+...` in subsequent invocations until that issue is fixed.

---

_Closed by @charliermarsh on 2024-03-12 14:45_

---

_Label `duplicate` added by @zanieb on 2024-03-12 14:46_

---

_Referenced in [astral-sh/uv#2596](../../astral-sh/uv/pulls/2596.md) on 2024-03-21 21:49_

---

_Comment by @zanieb on 2024-04-01 21:32_

Hi! This should be addressed in the latest release ([0.1.27](https://github.com/astral-sh/uv/releases/tag/0.1.27)) via #2596.

Let us know if you have any problems.

---
