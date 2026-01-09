---
number: 6363
title: Split/Remove Backward tokenization
type: issue
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2023-08-05T07:39:51Z
updated_at: 2023-09-21T00:54:47Z
url: https://github.com/astral-sh/ruff/issues/6363
synced_at: 2026-01-07T13:12:15-06:00
---

# Split/Remove Backward tokenization

---

_Issue opened by @MichaReiser on 2023-08-05 07:39_

The `SimpleTokenizer` supports backward lexing. The implementation will break when supporting Python 3.12's new F-String parsing because comments can now appear in parts that appear to be strings.

```
f"test{more  # quote
}"
```

My preferred option would be to remove backward lexing altogether. But I'm unsure how to support `is_parenthesized_expression` without it in the formatter. The problem is that we need to look back from the start of the expression. One option I could think of is to integrate the parenthesize detection into the `CommentsVisitor` where we already track the parent nodes (necessary to avoid mistaking `a` as a parenthesized expression in `call(a)`), and we could store the last start/position and start lexing from there. This would require skipping over some tokens, which I'm not sure we can handle.

The other alternative is to split the `SimpleTokenizer` into one implementation that only supports forward lexing and a `BackwardTokenizer` that supports backward lexing, but takes the `CommentRanges` as a second argument. 

CC: @dhruvmanila 

---

_Label `internal` added by @charliermarsh on 2023-08-08 18:54_

---

_Referenced in [astral-sh/ruff#6502](../../astral-sh/ruff/issues/6502.md) on 2023-08-24 05:55_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-12 05:00_

---

_Assigned to @konstin by @dhruvmanila on 2023-09-14 08:51_

---

_Unassigned @dhruvmanila by @dhruvmanila on 2023-09-14 08:51_

---

_Comment by @dhruvmanila on 2023-09-14 08:52_

/cc @konstin who's working on this, thanks for taking this up!

---

_Comment by @charliermarsh on 2023-09-21 00:54_

I believe this landed.

---

_Closed by @charliermarsh on 2023-09-21 00:54_

---
