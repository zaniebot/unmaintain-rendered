---
number: 12921
title: Respect --global Python pins during uv tool operations
type: issue
state: open
author: jtfmumm
labels:
  - enhancement
assignees: []
created_at: 2025-04-16T16:14:06Z
updated_at: 2025-05-23T07:05:16Z
url: https://github.com/astral-sh/uv/issues/12921
synced_at: 2026-01-10T01:25:26Z
---

# Respect --global Python pins during uv tool operations

---

_Issue opened by @jtfmumm on 2025-04-16 16:14_

We currently support pinning a global Python version via `uv python pin 3.12 --global`. But `uv tool` operations do not use this global pin. They should also respect the global pin (when a local Python version pin is not found in the working directory or an ancestor directory).

There is still design work to do here. It seems pretty clear that the above behavior should apply when installing a tool for the first time. But we have to determine how `uv tool` should behave on reinstalls and upgrades. Should it respect the global pin or the version it was originally installed with? If the latter, should we only ignore the global pin if the version was originally explicitly set with `--python`? 

It seems intuitive that installing with `--python` would ensure that future reinstalls and upgrades would use the same version. My inclination is that otherwise the global pin should be respected, even if it has changed since a tool was initially installed. That allows uv to be consistent in how it looks up the Python version to use (with `--python` always taking precedence). 

---

_Label `enhancement` added by @jtfmumm on 2025-04-16 16:14_

---

_Comment by @zanieb on 2025-04-16 16:19_

I would maybe rephrase as "Respect `--global` Python pins during `uv tool` operations"? It's also ignored during `uvx`.

We could probably support `uv tool run` easier than `uv tool install`. The latter has more complexity per the brief discussion at https://github.com/astral-sh/uv/pull/12115#discussion_r1992222278

---

_Renamed from "Support global Python pin for uv tool install" to "Respect --global Python pins during uv tool operations" by @jtfmumm on 2025-04-16 18:28_

---

_Referenced in [astral-sh/uv#8135](../../astral-sh/uv/issues/8135.md) on 2025-04-17 12:01_

---

_Comment by @mikelei8291 on 2025-05-22 21:24_

Why does the documentation for [`uv python pin --global`](https://docs.astral.sh/uv/reference/cli/#uv-python-pin--global) already state that commands like `uv tool install` would use the pin, when they actually do not?

> Unlike local version pins, this version is used as the default for commands that mutate global state, like `uv tool install`.

It's really confusing and frustrating when the program doesn't do what the documentation says. If this isn't going to be fixed soon, I think it's better to just remove this line first and add it back later when the issue is actually fixed.

---

_Referenced in [astral-sh/uv#13611](../../astral-sh/uv/pulls/13611.md) on 2025-05-23 05:45_

---

_Comment by @jtfmumm on 2025-05-23 07:05_

@mikelei8291 Thank you for catching this. I've fixed the documentation.

---

_Referenced in [astral-sh/uv#10711](../../astral-sh/uv/issues/10711.md) on 2025-06-17 06:20_

---

_Referenced in [astral-sh/uv#14112](../../astral-sh/uv/pulls/14112.md) on 2025-06-18 23:43_

---
