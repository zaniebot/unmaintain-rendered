```yaml
number: 6062
title: "Formatter: Allow comments between `and`/`or` and the right hand side"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-25T10:31:08Z
updated_at: 2023-09-12T06:39:59Z
url: https://github.com/astral-sh/ruff/issues/6062
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: Allow comments between `and`/`or` and the right hand side

---

_Issue opened by @konstin on 2023-07-25 10:31_

The following input is stable with black ...
```python
if (
    a
    # own line comment between left hand side and `and`
    and
    # own line comment between `and` and right hand side
    b
):
    pass
```
... but we move the second own line comment before the `and`:
```python
if (
    a
    # own line comment between left hand side and `and`
    # own line comment between `and` and right hand side
    and b
):
    pass
```
The behavior is the same with `or`, but `+` works, i.e. other binary expressions work but not boolean expressions.

---

_Label `formatter` added by @konstin on 2023-07-25 10:31_

---

_Comment by @MichaReiser on 2023-07-31 16:19_

We may want to generalize the formatting of all *BinaryLike* nodes to achieve consistent formatting.

---

_Comment by @MichaReiser on 2023-08-16 07:14_

I actually prefer our formatting over black's and it preserves the comment ordering. But we should align the comment placement for all binary like expressions.

---

_Comment by @zanieb on 2023-08-16 17:29_

I agree with Micha that our formatting is more readable at the expense of possibly overriding user intent. I don't see what extra context can be given by having the comment after the operator though.

---

_Comment by @MichaReiser on 2023-08-17 06:16_

I guess the challenge with the current formatting is how to handle trailing `and` comments and preserve the comment line position and order: [Playground](https://play.ruff.rs/de5c3b05-692f-4f1a-8002-b45282653f3c)

I think my preferred formatting is:

```python
if (
    a
    # own line comment between left hand side and `and`
    # trailing
    # own line comment between `and` and right hand side
    and b
):
    pass
```

It preserves the order, but changes `trailing` to an own linecomment. But I think this overall looks better.

Edit: Similar issue https://github.com/astral-sh/ruff/issues/6647



---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-17 09:43_

---

_Comment by @MichaReiser on 2023-08-29 14:45_

And this is another funny one CC: @charliermarsh You best run our formatter on `django` and take a look at the diff. There are plenty of examples.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-30 04:27_

---

_Comment by @charliermarsh on 2023-08-30 04:27_

I can take it on.

---

_Comment by @MichaReiser on 2023-08-30 10:09_

Thank you @charliermarsh 

If possible, could we align the comment handling with the binary expression formatting? It's very likely that I have to unify the two implementations and that would be significantly easier if they roughly work the same.

---

_Comment by @MichaReiser on 2023-09-06 15:52_

It turns out that the string formatting doesn't overlap with `and` `or` formatting. It only affects binary and comparison expressions. That means this is still up for grabs. 

Overall, the binary / compare formatting seems very reasonable. Maybe we can mimick that formatting for boolean expressions

---

_Comment by @MichaReiser on 2023-09-07 08:02_

@charliermarsh do you plan to work on this next week? If not, @konstin would you be interested in taking a look. It's our biggest difference in Django by now or I'll try to have a look since it's related to binary expression formatting.

---

_Unassigned @charliermarsh by @charliermarsh on 2023-09-07 10:17_

---

_Comment by @charliermarsh on 2023-09-07 10:17_

@konstin - please go ahead! Haven’t started on it at all.

---

_Assigned to @konstin by @konstin on 2023-09-08 09:19_

---

_Comment by @MichaReiser on 2023-09-08 17:30_

@konstin sorry for the forth and back. I consider rewriting the boolean expression formatting to use the `BinaryLike` formatting (the same logic as the binary and compare expressions use) to fix #7004 just once rather than in two places. Maybe... just maybe, it fixes this difference too

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-11 11:38_

---

_Unassigned @konstin by @MichaReiser on 2023-09-11 11:38_

---

_Closed by @MichaReiser on 2023-09-12 06:39_

---
