---
number: 18622
title: Using LiteralString type with S605
type: issue
state: open
author: Kajiih
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-06-11T09:24:48Z
updated_at: 2025-06-11T13:52:20Z
url: https://github.com/astral-sh/ruff/issues/18622
synced_at: 2026-01-07T13:12:16-06:00
---

# Using LiteralString type with S605

---

_Issue opened by @Kajiih on 2025-06-11 09:24_

### Summary

I think S605 and similar rules should ideally detect cases where a `LiteralString` is passed rather than an actual string literal.

This might be out of the scope of ruff because it seems like it requires static type analysis, but with incoming `ty` can such a feature be implemented?

Example here:
https://play.ruff.rs/bfdec0ac-190b-4b10-b828-524229dffebb

### Version

ruff 0.11.13

---

_Label `rule` added by @ntBre on 2025-06-11 10:14_

---

_Label `type-inference` added by @ntBre on 2025-06-11 10:14_

---

_Comment by @MichaReiser on 2025-06-11 13:52_

See, https://docs.python.org/3/library/typing.html#typing.LiteralString

I think this is something that Ruff could already support today

Long term, the use of template's is probabl the better solution over `LiteralString`.

---
