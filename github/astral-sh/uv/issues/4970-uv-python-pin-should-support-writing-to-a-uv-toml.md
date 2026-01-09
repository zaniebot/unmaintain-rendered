---
number: 4970
title: "`uv python pin` should support writing to a `uv.toml` or `pyproject.toml`"
type: issue
state: open
author: zanieb
labels:
  - needs-design
assignees: []
created_at: 2024-07-10T17:05:57Z
updated_at: 2025-08-12T05:52:08Z
url: https://github.com/astral-sh/uv/issues/4970
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv python pin` should support writing to a `uv.toml` or `pyproject.toml`

---

_Issue opened by @zanieb on 2024-07-10 17:05_

We need to design the schema for this data. Probably at `uv.python-version` or `uv.python.default-version`.

---

_Label `needs-design` added by @zanieb on 2024-07-10 17:05_

---

_Label `preview` added by @zanieb on 2024-07-10 17:05_

---

_Referenced in [astral-sh/uv#4971](../../astral-sh/uv/issues/4971.md) on 2024-07-10 17:06_

---

_Referenced in [astral-sh/uv#4963](../../astral-sh/uv/issues/4963.md) on 2024-07-11 14:43_

---

_Comment by @T-256 on 2024-07-11 15:06_

Then, should it also be top-level command `uv pin`? since it tries to manage project/workspace directly.

---

_Comment by @zanieb on 2024-07-11 15:20_

That's a reasonable question. I'm not sure. I don't love it at the top-level since it's not obvious what we're pinning.

---

_Referenced in [astral-sh/uv#5126](../../astral-sh/uv/issues/5126.md) on 2024-07-16 20:03_

---

_Referenced in [astral-sh/uv#4791](../../astral-sh/uv/pulls/4791.md) on 2024-07-19 14:25_

---

_Referenced in [astral-sh/uv#5215](../../astral-sh/uv/pulls/5215.md) on 2024-07-19 16:42_

---

_Comment by @zanieb on 2024-07-24 02:40_

I duped myself, https://github.com/astral-sh/uv/issues/4359 covers reading from the configuration file.

---

_Referenced in [astral-sh/uv#5592](../../astral-sh/uv/pulls/5592.md) on 2024-07-30 14:17_

---

_Assigned to @zanieb by @charliermarsh on 2024-07-30 16:13_

---

_Comment by @charliermarsh on 2024-07-30 16:13_

(At least figure out the rough design prior to the release.)

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Referenced in [astral-sh/uv#6780](../../astral-sh/uv/issues/6780.md) on 2024-08-28 23:08_

---

_Referenced in [astral-sh/uv#6821](../../astral-sh/uv/issues/6821.md) on 2024-08-29 16:44_

---

_Referenced in [astral-sh/uv#7429](../../astral-sh/uv/issues/7429.md) on 2024-09-16 22:25_

---

_Referenced in [astral-sh/uv#8247](../../astral-sh/uv/issues/8247.md) on 2024-11-14 15:06_

---

_Referenced in [astral-sh/uv#9494](../../astral-sh/uv/issues/9494.md) on 2024-11-28 10:13_

---

_Comment by @edmorley on 2024-12-11 23:37_

I'm slightly concerned this will lead to more fragmentation of where third party ecosystems (such as Heroku, GHA setup-python, Dependabot, ...) will need to check for Python versions when bootstrapping an environment.

If there was a field in `pyproject.toml` that was guaranteed in the upstream spec to be a single Python version (either `X.Y.Z` or `X.Y`) rather than a multiple major version range, then I'd be fine with the ecosystem consolidating around that, but as-is the existing `pyproject.toml` `requires-python` field isn't suitable for bootstrapping purposes, and so the `.python-version` file is the next best thing available - and pretty widely accepted at this point.

---

_Comment by @zanieb on 2024-12-12 01:31_

> ... as-is the existing pyproject.toml requires-python field isn't suitable for bootstrapping purposes, and so the .python-version file is the next best thing available - and pretty widely accepted at this point.

Agreed. That's why we've delayed implementing this. Unfortunately, we're getting quite a bit of confusion from users. I'm not sure how we'll address that.

---

_Referenced in [astral-sh/uv#10204](../../astral-sh/uv/issues/10204.md) on 2024-12-27 19:45_

---

_Referenced in [astral-sh/uv#11697](../../astral-sh/uv/issues/11697.md) on 2025-02-21 13:08_

---

_Referenced in [astral-sh/uv#12427](../../astral-sh/uv/issues/12427.md) on 2025-04-01 18:20_

---

_Referenced in [astral-sh/uv#14393](../../astral-sh/uv/issues/14393.md) on 2025-07-02 02:08_

---

_Comment by @trondhindenes on 2025-08-12 05:50_

just to opine on this: The number one issue I find when helping fellow python devs un-tangling their botched python dev setups, is the presence of a ".python-version" file in some directory, since it has potential far-reaching effects. If the user happens to have "pyenv" installed, then this file in the current (or parent) dir(s) will replace the default python interpreter, leading to expected commands not being available etc. 

It is therefore problematic for uv to create and/or use this file, as it has side-efects far outside of uv itself (think developer just cding into a dir in their terminal in order to run some command). 
The fact that ".python-version" is "widely acceptable" is the exact reason uv should _not_ go anywhere near it - it should aim to minimize the risk affecting other aspects of a developer's environment. I would ask maintaines to consider at least supporting that an alternate filename like `.uv.python-version` or similar is supported as a fallback if ".python-version" is not found.

---
