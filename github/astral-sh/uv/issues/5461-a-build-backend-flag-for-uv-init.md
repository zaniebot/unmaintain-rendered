---
number: 5461
title: "A build backend flag for `uv init`"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2024-07-25T22:27:41Z
updated_at: 2024-07-29T14:00:13Z
url: https://github.com/astral-sh/uv/issues/5461
synced_at: 2026-01-07T13:12:17-06:00
---

# A build backend flag for `uv init`

---

_Issue opened by @konstin on 2024-07-25 22:27_

The default build backend, setuptools, is somewhat archaic and has undesirable behaviors, such as creating an `.egg-info` directory. We should add an `--build-backend` flag to `uv init` which some popular choices, such as hatch and flit.

---

_Label `enhancement` added by @konstin on 2024-07-25 22:27_

---

_Label `needs-decision` added by @konstin on 2024-07-25 22:27_

---

_Comment by @charliermarsh on 2024-07-25 22:29_

I'm wondering if we should just use a different default, to match Rye?

---

_Comment by @konstin on 2024-07-26 15:39_

Flit or hatch seem to be the popular choices, both have been working well for me.

---

_Comment by @charliermarsh on 2024-07-26 17:33_

FWIW, Hatchling is about an order of magnitude more popular than Flit (and setuptools is about an order of magnitude more popular than Hatchling), by downloads.

---

_Comment by @helderco on 2024-07-27 23:01_

I vote for hatchling.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-28 21:30_

---

_Referenced in [astral-sh/uv#5527](../../astral-sh/uv/pulls/5527.md) on 2024-07-28 21:30_

---

_Comment by @AndreuCodina on 2024-07-29 11:58_

IMO `--build-backend` isn't a friendly name. I create libraries or packages with Python, no backends.

I could suggest `--package-backend`, `--package-build-system`, `--package-builder` or `--package-something`.

---

_Comment by @AlexWaygood on 2024-07-29 12:34_

@AndreuCodina -- "build backend" is the standard term here, however. If you google "build backend Python", you get more helpful results introducing you to the concept than if you google "package backend Python".

---

_Comment by @bluss on 2024-07-29 13:03_

Is there a build backend that can interoperate well with uv's support for relative paths and workspaces?

---

_Closed by @charliermarsh on 2024-07-29 14:00_

---
