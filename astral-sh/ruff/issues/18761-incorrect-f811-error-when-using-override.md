```yaml
number: 18761
title: Incorrect F811 error when using override
type: issue
state: closed
author: JoBrad
labels:
  - question
assignees: []
created_at: 2025-06-18T16:49:58Z
updated_at: 2025-07-24T12:00:56Z
url: https://github.com/astral-sh/ruff/issues/18761
synced_at: 2026-01-12T15:54:56Z
```

# Incorrect F811 error when using override

---

_@JoBrad_

### Summary

Issue: For the code below, ruff is flagging the 2nd and 3rd definitions with code F811 (redefined-while-unused).

Expectation: I would expect ruff to see the @override decorator, and avoid flagging this as a redefinition.

Playground link: https://play.ruff.rs/d67885d5-1e65-4901-acd4-3409e37779ac


### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Renamed from "Incorrect error when using override" to "Incorrect F811 error when using override" by @JoBrad on 2025-06-18 16:50_

---

_Comment by @ntBre on 2025-06-18 17:36_

I think you're supposed to use [`typing.overload`](https://typing.python.org/en/latest/spec/overload.html#overload-definitions) not `override` for this: https://play.ruff.rs/1800b113-560e-4fc1-9898-37971d0fbe33

With that change, F811 should be suppressed.

---

_Label `question` added by @ntBre on 2025-06-18 17:37_

---

_Closed by @MichaReiser on 2025-07-24 12:00_

---
