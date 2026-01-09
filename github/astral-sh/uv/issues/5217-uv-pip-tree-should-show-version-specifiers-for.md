---
number: 5217
title: "`uv pip tree` should show version specifiers for dependencies."
type: issue
state: closed
author: Enayaaa
labels:
  - enhancement
  - help wanted
  - cli
assignees: []
created_at: 2024-07-19T13:38:59Z
updated_at: 2024-07-20T18:31:18Z
url: https://github.com/astral-sh/uv/issues/5217
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv pip tree` should show version specifiers for dependencies.

---

_Issue opened by @Enayaaa on 2024-07-19 13:38_

With `uv==0.2.26` it is not possible to see version specifiers in dependency graph.

I have used `pipdeptree` to find why some package is staying back. With version specifiers in the dependency tree you could find what package that depends on `foo` causes some dependency shared between multiple packages to stay back on an older version.
```console
$  pipdeptree -lrp apeye
apeye==1.4.1
├── pypi-json==0.4.0 [requires: apeye>=1.1.0]
│   └── relic-radar==0.1.dev62+g0574a5f.d20240716093527 [requires: pypi-json>=0.3,<1]
└── relic-radar==0.1.dev62+g0574a5f.d20240716093527 [requires: apeye>=0.5,<2]
```
With `pipdeptree` they are shown inside brackets after the version.

---

_Comment by @charliermarsh on 2024-07-19 13:44_

That's a neat idea.

---

_Label `enhancement` added by @charliermarsh on 2024-07-19 13:51_

---

_Label `cli` added by @charliermarsh on 2024-07-19 13:51_

---

_Label `help wanted` added by @charliermarsh on 2024-07-19 13:51_

---

_Comment by @zanieb on 2024-07-19 14:02_

This was actually in the original implementation and I asked it to be made opt-in: https://github.com/astral-sh/uv/pull/3859#issuecomment-2181683137 

I still think it's probably much noise for the default output, but am happy to have a flag that adds it? cc @ChannyClaus 

---

_Comment by @charliermarsh on 2024-07-19 14:03_

Yeah opt-in is cool with me. That makes sense.

---

_Comment by @ChannyClaus on 2024-07-19 14:31_

i can do this sometime this weekend 🌵 

---

_Referenced in [astral-sh/uv#5240](../../astral-sh/uv/pulls/5240.md) on 2024-07-20 06:43_

---

_Closed by @charliermarsh on 2024-07-20 18:31_

---

_Closed by @charliermarsh on 2024-07-20 18:31_

---
