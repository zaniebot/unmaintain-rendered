---
number: 6588
title: "Formatter: Comprehension leading if comments"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-15T05:41:35Z
updated_at: 2023-09-29T09:45:04Z
url: https://github.com/astral-sh/ruff/issues/6588
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Comprehension leading if comments

---

_Issue opened by @MichaReiser on 2023-08-15 05:41_

This is not a bug per se but something I found rather confusing. The comprehension comment placement associates leading and trailing comments on the `if`s as dangling comments of the condition node:

```
// Handle comments inside comprehensions, e.g.
//
// ```python
// [
//      a
//      b
//      c
//      # leading becomes dangling on if
//      if  # trailing becomes dangling on if
//      d
// ]
// ```
```

https://github.com/astral-sh/ruff/blob/96d310fbab0112f8ab899f6d665f79a5aecdafd5/crates/ruff_python_formatter/src/comments/placement.rs#L1244-L1369

This is confusing because nodes that otherwise cannot have dangling comments now suddenly have dangling comments. I ran into this because I added assertions to `fmt_dangling_comments` in `ExprName` and `ExprCompare` to verify that the dangling comments are empty (because that's what the comment says) and got surprised by the assertions failing. 

The more common approach is to make the comments dangling comments on the comprehension and then find them again during formatting. This is probably a bit more work. Let's see how it turns out.

---

_Label `formatter` added by @MichaReiser on 2023-08-15 05:41_

---

_Label `help wanted` added by @MichaReiser on 2023-08-15 05:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-16 17:37_

---

_Comment by @charliermarsh on 2023-08-16 17:37_

I can take a look at this.

---

_Unassigned @charliermarsh by @charliermarsh on 2023-08-17 01:24_

---

_Comment by @charliermarsh on 2023-08-17 01:24_

Gonna prioritize some other things over this but agree we should do it.

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-17 09:44_

---

_Referenced in [astral-sh/ruff#7066](../../astral-sh/ruff/issues/7066.md) on 2023-09-02 18:35_

---

_Referenced in [astral-sh/ruff#7605](../../astral-sh/ruff/issues/7605.md) on 2023-09-27 13:49_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-28 08:40_

---

_Referenced in [astral-sh/ruff#7693](../../astral-sh/ruff/pulls/7693.md) on 2023-09-28 09:32_

---

_Closed by @MichaReiser on 2023-09-29 09:45_

---
