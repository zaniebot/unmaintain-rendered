```yaml
number: 3140
title: feat request to resolve rule descrepancy
type: issue
state: closed
author: upstartjohnvandivier
labels:
  - bug
assignees: []
created_at: 2023-02-22T19:21:44Z
updated_at: 2023-02-22T22:37:23Z
url: https://github.com/astral-sh/ruff/issues/3140
synced_at: 2026-01-12T15:54:43Z
```

# feat request to resolve rule descrepancy

---

_@upstartjohnvandivier_

there is a rule descrepancy in E7111 vs F632 we can't comply with both
one says i must `0.1 is not None`
the other says i must `0.1 != None`

may i request a `Ruff-specific rules (RUF)` that reconciles these? it basically would say "use != in constant comparisons that don't involve None else use is not None syntax`

---

_Comment by @charliermarsh on 2023-02-22 22:24_

I think it has to go the other way, because `0.1 is not None` is a syntax warning:

```
>>> 0.1 is not None
<stdin>:1: SyntaxWarning: "is not" with a literal. Did you mean "!="?
```

We should allow `0.1 != None` (although it seems strange to have code that compares two literals like that :))

---

_Label `bug` added by @charliermarsh on 2023-02-22 22:24_

---

_Closed by @charliermarsh on 2023-02-22 22:37_

---
