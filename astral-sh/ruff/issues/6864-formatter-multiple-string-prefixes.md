---
number: 6864
title: "formatter: multiple string prefixes"
type: issue
state: closed
author: davidszotten
labels: []
assignees: []
created_at: 2023-08-25T07:16:10Z
updated_at: 2023-08-25T07:58:28Z
url: https://github.com/astral-sh/ruff/issues/6864
synced_at: 2026-01-10T01:22:46Z
---

# formatter: multiple string prefixes

---

_Issue opened by @davidszotten on 2023-08-25 07:16_

Not looked into this much yet, but i think we aren't handling multiple string prefixes correctly, e.g.

input: `rb"Not-so-tricky \"quote"`
black: `rb"Not-so-tricky \"quote"`
ruff: `rb'Not-so-tricky "quote'`

---

_Referenced in [astral-sh/ruff#6865](../../astral-sh/ruff/pulls/6865.md) on 2023-08-25 07:26_

---

_Closed by @konstin on 2023-08-25 07:58_

---
