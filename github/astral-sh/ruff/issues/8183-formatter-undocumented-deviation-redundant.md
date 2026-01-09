---
number: 8183
title: "Formatter undocumented deviation: Redundant parentheses in boolean expression can lead to extra line breaks"
type: issue
state: closed
author: henribru
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-24T20:46:23Z
updated_at: 2023-10-26T06:29:00Z
url: https://github.com/astral-sh/ruff/issues/8183
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter undocumented deviation: Redundant parentheses in boolean expression can lead to extra line breaks

---

_Issue opened by @henribru on 2023-10-24 20:46_

[Black:](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADtAHtdAD2IimZxl1N_WlbvK5V9KEd0suDTtKdXyv9jIpYCegD3gYD7EyeAgiILbu4HLFJcJOGomsGIui2j4fvxg-ubYGm00rJ3POd7Otj7BsS8qKAOPqj5RYhHkKKWdavWCbQBVtBbfaMZwMbeFK26F0ek2yy-Pe023i3gAQ-rAAAATqNMSNT2D50AAZcB7gEAAKqHNMmxxGf7AgAAAAAEWVo=)
```
def foo():
    while (
        not (aaaaaaaaaaaaaaaaaaaaa(bbbbbbbb, ccccccc)) and dddddddddd < eeeeeeeeeeeeeee
    ):
        pass
```

[Ruff:](https://play.ruff.rs/485d6e0e-16e1-4b64-8756-8cd3f62c5f4f?secondary=Format)
```
def foo():
    while not (
        aaaaaaaaaaaaaaaaaaaaa(bbbbbbbb, ccccccc)
    ) and dddddddddd < eeeeeeeeeeeeeee:
        pass
```

---

_Label `formatter` added by @MichaReiser on 2023-10-24 21:25_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-24 21:25_

---

_Comment by @MichaReiser on 2023-10-25 09:00_

Could this be related/due to #8100?

---

_Comment by @MichaReiser on 2023-10-25 10:10_

I think the issue here is that we set `first` in `CanOmitOptionalParenthesesVisitor` to the `UnaryExpr.value` instead of the unary expression itself. I think we have the same problem for other nodes that start with a token (if expression, dict etc)

---

_Comment by @MichaReiser on 2023-10-25 10:32_

Okay I think I understand it now... It is an issue with `can_omit_parentheses`. Black uses different rules to test if the first or last node have parentheses. 

* first: Needs to start with parentheses. That excludes call expressions. 
* last: Needs to end with parentheses. Call, lists, etc

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-25 10:40_

---

_Referenced in [astral-sh/ruff#8238](../../astral-sh/ruff/pulls/8238.md) on 2023-10-26 03:31_

---

_Label `bug` added by @MichaReiser on 2023-10-26 03:31_

---

_Closed by @MichaReiser on 2023-10-26 06:28_

---
