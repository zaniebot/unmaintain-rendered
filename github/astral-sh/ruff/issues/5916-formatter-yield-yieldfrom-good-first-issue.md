---
number: 5916
title: "Formatter: `Yield` / `YieldFrom` (good first issue)"
type: issue
state: closed
author: MichaReiser
labels:
  - good first issue
  - formatter
assignees: []
created_at: 2023-07-20T10:57:15Z
updated_at: 2023-07-21T12:07:53Z
url: https://github.com/astral-sh/ruff/issues/5916
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: `Yield` / `YieldFrom` (good first issue)

---

_Issue opened by @MichaReiser on 2023-07-20 10:57_

* [ ] `yield`
* [ ] `yield..from`

---

_Referenced in [astral-sh/ruff#4798](../../astral-sh/ruff/issues/4798.md) on 2023-07-20 10:57_

---

_Comment by @MichaReiser on 2023-07-20 11:05_

@qdegraaf has shown interest in working on this

You can find the dummy implementation here 

https://github.com/astral-sh/ruff/blob/76e9ce6dc048cfa764bc50c498969d39e9501a57/crates/ruff_python_formatter/src/expression/expr_yield.rs#L1-L25

I recommend taking a look at the `raise` stmt PR #5595 and/or formatting of [`await` expressions](https://github.com/astral-sh/ruff/blob/76e9ce6dc048cfa764bc50c498969d39e9501a57/crates/ruff_python_formatter/src/expression/expr_await.rs#L5-L4) because I assume the formatting would be similar. 

Oh, and I should not forget the [documentation](https://github.com/astral-sh/ruff/blob/76e9ce6dc048cfa764bc50c498969d39e9501a57/crates/ruff_python_formatter/README.md)

---

_Label `good first issue` added by @zanieb on 2023-07-20 13:16_

---

_Referenced in [astral-sh/ruff#5921](../../astral-sh/ruff/pulls/5921.md) on 2023-07-20 14:34_

---

_Label `formatter` added by @konstin on 2023-07-20 14:54_

---

_Closed by @MichaReiser on 2023-07-21 12:07_

---
