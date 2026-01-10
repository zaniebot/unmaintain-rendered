---
number: 5673
title: "Formatter: Expression subscript gets additional whitespace where black doesn't insert any"
type: issue
state: closed
author: konstin
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-07-11T07:40:26Z
updated_at: 2023-07-20T15:05:23Z
url: https://github.com/astral-sh/ruff/issues/5673
synced_at: 2026-01-10T01:22:44Z
---

# Formatter: Expression subscript gets additional whitespace where black doesn't insert any

---

_Issue opened by @konstin on 2023-07-11 07:40_

We format
```python
parts[1:-1]
```
as
```
parts[1 : -1]
```
We need to change the logic to treat `-1` like a normal constant instead of a complex expression that needs whitespace

---

_Label `formatter` added by @konstin on 2023-07-11 07:40_

---

_Comment by @MichaReiser on 2023-07-11 07:52_

> We need to change the logic to treat -1 like a normal constant instead of a complex expression that needs whitespace

Is it specific to `-1` or does it apply to all unary expressions (`+` and `-`) that have a numeric constant?

---

_Comment by @konstin on 2023-07-11 09:16_

both plus and minus work, `not` doesn't, for other operators i'd have to check the black source

---

_Label `help wanted` added by @MichaReiser on 2023-07-11 10:11_

---

_Referenced in [astral-sh/ruff#5922](../../astral-sh/ruff/pulls/5922.md) on 2023-07-20 14:58_

---

_Closed by @konstin on 2023-07-20 15:05_

---
