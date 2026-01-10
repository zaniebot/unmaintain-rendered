```yaml
number: 16766
title: "E262 separation of \"too many\" \"not enough\""
type: issue
state: closed
author: beauxq
labels:
  - question
assignees: []
created_at: 2025-03-15T18:46:19Z
updated_at: 2025-03-17T08:18:46Z
url: https://github.com/astral-sh/ruff/issues/16766
synced_at: 2026-01-10T11:09:57Z
```

# E262 separation of "too many" "not enough"

---

_Issue opened by @beauxq on 2025-03-15 18:46_

### Summary

E262 is reporting both:
- cases where there are fewer than 1 spaces after the `#`
  - `#comment`
- cases where there are more than 1 spaces after the `#`
  - `#   comment`

I would like a report when there are fewer than 1 spaces after the `#` but no report when there are more than 1 spaces.

Can this be configurable? or separated into different rules?

---

_Label `rule` added by @ntBre on 2025-03-15 20:01_

---

_Label `configuration` added by @ntBre on 2025-03-15 20:01_

---

_Comment by @MichaReiser on 2025-03-16 09:08_

Can you tell me more about why you want to allow more than one space?

I don't think it makes sense to make this rule configurable because the goal of the pycodestyle rules is to reflect the recommendations from PEP8 and that is:

> Per [PEP 8](https://peps.python.org/pep-0008/#comments), inline comments should start with a # and a single space.

---

_Label `rule` removed by @MichaReiser on 2025-03-16 09:09_

---

_Label `configuration` removed by @MichaReiser on 2025-03-16 09:09_

---

_Label `question` added by @MichaReiser on 2025-03-16 09:09_

---

_Comment by @beauxq on 2025-03-16 22:00_

The pattern in the code that I was trying to bring ruff into:
```python
    {
        a: b,  #   1 -  25
        c: d,  #  26 -  50
        e: f,  #  51 -  75
        g: h,  #  76 - 100
        i: j,  # 101 - 125
    }
```
PEP8 even has an exception for the single space in block comments:
> Each line of a block comment starts with a # and a single space (unless it is indented text inside the comment).

The way I see it, a single comment could be both an inline comment and a block comment at the same time. That's what this example shows.

---

_Comment by @MichaReiser on 2025-03-17 08:16_

Thanks for sharing. I can see how this formatting is desirable but it's out of this rule's scope (which is PEP8 centered).

I don't have a good solution for you other than disabling the rule for the entire project or this specific file. We may introduce support for suppressing a rule for an entire block (https://github.com/astral-sh/ruff/issues/3711) which could be useful to you if this isn't a very common pattern.

---

_Closed by @MichaReiser on 2025-03-17 08:16_

---

_Closed by @MichaReiser on 2025-03-17 08:17_

---
