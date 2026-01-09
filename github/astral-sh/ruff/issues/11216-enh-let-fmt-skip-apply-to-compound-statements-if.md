---
number: 11216
title: "[ENH] Let `fmt: skip` apply to compound statements, if they share a line."
type: issue
state: closed
author: randolf-scholz
labels:
  - bug
  - suppression
  - formatter
assignees: []
created_at: 2024-04-30T16:55:50Z
updated_at: 2025-11-18T18:02:10Z
url: https://github.com/astral-sh/ruff/issues/11216
synced_at: 2026-01-07T13:12:15-06:00
---

# [ENH] Let `fmt: skip` apply to compound statements, if they share a line.

---

_Issue opened by @randolf-scholz on 2024-04-30 16:55_

```python
if True: x = 0  # fmt: skip

def foo(self, lorem, ipsum, dolor, sit, amet, consectetur, adipiscing, elit, sed, do): ...  # fmt: skip
```

Gets reformatted to

```python
if True:
    x = 0  # fmt: skip

def foo(
    self, lorem, ipsum, dolor, sit, amet, consectetur, adipiscing, elit, sed, do
): ...  # fmt: skip
```

Which is because `fmt: skip` only applies to the preceding `statement`. However, it would be useful to apply the `fmt: skip` to the whole [compound statement](https://docs.python.org/3/reference/compound_stmts.html) in this context. For instance, when formatting `@overload` decorated functions.

---

_Label `bug` added by @zanieb on 2024-04-30 16:58_

---

_Label `suppression` added by @zanieb on 2024-04-30 16:58_

---

_Label `formatter` added by @zanieb on 2024-04-30 16:58_

---

_Label `bug` removed by @charliermarsh on 2024-04-30 18:31_

---

_Label `bug` added by @charliermarsh on 2024-04-30 18:32_

---

_Comment by @MichaReiser on 2024-05-01 07:07_

Yeah that makes sense to me. I'm not entirely sure how to implement this yet. I see two approaches:

1. Add special handling for `fmt: skip` and `fmt: off` (and I think `yapf:disable`) comments in `placement.rs` that come at the end of a clause header. Instead of associating them with the preceding statement, associate them with the clause header (but only if both are on the same line). 
2. Pass the comments of the first simple statement to `FormatClauseHeader` and make the decision there. 

The good news, the unused formatter suppression comment lint rule seems to already accept this.

---

_Referenced in [astral-sh/ruff#11430](../../astral-sh/ruff/issues/11430.md) on 2024-06-21 23:39_

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-09-16 16:19_

---

_Referenced in [astral-sh/ruff#13487](../../astral-sh/ruff/pulls/13487.md) on 2024-09-23 20:40_

---

_Comment by @bnkc on 2024-09-23 20:42_

Hi @MichaReiser I did a first pass on this issue. Not sure if my approach is what we were looking for but I'd be happy to change direction. Let me know 👍 https://github.com/astral-sh/ruff/pull/13487


---

_Referenced in [astral-sh/ruff#17331](../../astral-sh/ruff/issues/17331.md) on 2025-04-10 12:13_

---

_Comment by @AlexanderHott on 2025-05-19 15:50_

Any update on this issue/PR?

---

_Comment by @michelkluger on 2025-05-20 07:07_

it is also ignoring this

```
    # fmt: off
    "query, expected",
    [
        ("potato", {"text": "potato", "highlight": "<b>potato</b>", "item_type": "product"}),
        ("oxirane", {"text": "epichlorohydrin", "highlight": "epichlorohydrin", "item_type": "product"}),
        ("7722-76-1", {"text": "7722-76-1", "highlight": "<b>7722-76-1</b>", "item_type": "cas_number"}),
        ("potat", {"text": "potato", "highlight": "<b>potato</b>", "item_type": "product"}),
        ("potota", {"text": "potato", "highlight": "<b>potato</b>", "item_type": "product"}),
        ("totato", {"text": "potato", "highlight": "<b>potato</b>", "item_type": "product"}),
    ],
    ids=["exact", "synonym", "cas", "typo_missing", "typo_transposed", "typo_misplaced"]
)
    # fmt: on
```

---

_Referenced in [astral-sh/ruff#18658](../../astral-sh/ruff/issues/18658.md) on 2025-06-13 11:40_

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:37_

---

_Referenced in [astral-sh/ruff#20633](../../astral-sh/ruff/pulls/20633.md) on 2025-09-29 19:48_

---

_Referenced in [astral-sh/ruff#20794](../../astral-sh/ruff/pulls/20794.md) on 2025-10-13 18:10_

---

_Closed by @dylwil3 on 2025-11-18 18:02_

---
