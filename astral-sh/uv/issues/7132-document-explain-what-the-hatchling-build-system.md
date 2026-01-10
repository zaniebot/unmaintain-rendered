---
number: 7132
title: "Document/explain what the \"hatchling\" build-system is about"
type: issue
state: closed
author: gdamjan
labels:
  - question
assignees: []
created_at: 2024-09-06T17:40:13Z
updated_at: 2024-10-21T21:43:20Z
url: https://github.com/astral-sh/uv/issues/7132
synced_at: 2026-01-10T01:24:11Z
---

# Document/explain what the "hatchling" build-system is about

---

_Issue opened by @gdamjan on 2024-09-06 17:40_

`uv init` adds the hatchling build system to pyproject.toml.

please write a short blurb (a sentence or two) what that means, and why it is needed.

(I personally have no idea what the role of `hatchling` is in the whole `uv` concept)

---

_Comment by @zanieb on 2024-09-06 17:44_

We don't do this by default anymore, but hatchling is a build backend which is required to build and install your project: https://packaging.python.org/en/latest/tutorials/packaging-projects/#choosing-a-build-backend

We chose hatchling because we think it's a reasonable option, but there are many other backends. Eventually, we'll write our own #3957.

Learn more about build-systems and uv projects at https://docs.astral.sh/uv/concepts/projects/#build-systems

---

_Label `question` added by @zanieb on 2024-09-06 17:44_

---

_Closed by @zanieb on 2024-10-21 21:43_

---

_Referenced in [BlockstreamResearch/bip-frost-dkg#83](../../BlockstreamResearch/bip-frost-dkg/pulls/83.md) on 2025-02-27 10:10_

---

_Referenced in [stanford-centaur/PyPantograph#105](../../stanford-centaur/PyPantograph/pulls/105.md) on 2025-04-30 01:53_

---
