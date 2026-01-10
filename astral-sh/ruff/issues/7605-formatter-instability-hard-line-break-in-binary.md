```yaml
number: 7605
title: "Formatter instability: hard line break in binary expression"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - formatter
  - accepted
assignees: []
created_at: 2023-09-22T16:37:13Z
updated_at: 2023-09-29T09:45:03Z
url: https://github.com/astral-sh/ruff/issues/7605
synced_at: 2026-01-10T11:09:49Z
```

# Formatter instability: hard line break in binary expression

---

_Issue opened by @charliermarsh on 2023-09-22 16:37_

Given:

```python
[
    1 for components in  # pylint: disable=undefined-loop-variable
    b +  # integer 1 may only have decimal 01-09
    c  # negative decimal
] 
```

We format as:

```python
[
    1
    for components in b  # pylint: disable=undefined-loop-variable  # integer 1 may only have decimal 01-09
    +
    c  # negative decimal
]
```

(Notice the hard line break between `+` and `c # negative decimal`.)

Which is then formatted as:

```python
[
    1
    for components in b  # pylint: disable=undefined-loop-variable  # integer 1 may only have decimal 01-09
    + c  # negative decimal
]

```

Sourced from https://github.com/astral-sh/ruff/issues/7590 (`A_1798643049678049848.py`).


---

_Label `bug` added by @charliermarsh on 2023-09-22 16:37_

---

_Label `formatter` added by @charliermarsh on 2023-09-22 16:37_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-09-22 16:37_

---

_Comment by @MichaReiser on 2023-09-22 16:44_

I updated the summary with a smaller reproduction

---

_Comment by @charliermarsh on 2023-09-22 16:46_

Thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-22 16:46_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-09-22 16:46_

---

_Comment by @charliermarsh on 2023-09-22 16:46_

Didn't mean to assign to myself :joy:

---

_Comment by @MichaReiser on 2023-09-27 13:49_

I think this is caused by https://github.com/astral-sh/ruff/issues/6588

The binary expression can have dangling comments and it formats them manually. But it's the comprehension that attaches a dangling comment to the binary expression, which messes up the formatting logic, which it intends to handle but the binary expression formatting can't know that. 

The solution here is that comprehension shouldn't attach dangling comments to other nodes. 

---

_Comment by @charliermarsh on 2023-09-27 13:51_

I can fix that.

---

_Comment by @MichaReiser on 2023-09-27 14:10_

The problematic comment placement are:

https://github.com/astral-sh/ruff/blob/865c89800ee7bc03ec43b87ce8c0f063a4f50a7d/crates/ruff_python_formatter/src/comments/placement.rs#L2124-L2131

Not related to this issue but has the same underlying problem
https://github.com/astral-sh/ruff/blob/865c89800ee7bc03ec43b87ce8c0f063a4f50a7d/crates/ruff_python_formatter/src/comments/placement.rs#L2158-L2164

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 16:00_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-09-27 18:51_

---

_Label `accepted` added by @charliermarsh on 2023-09-27 18:51_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-28 07:56_

---

_Closed by @MichaReiser on 2023-09-29 09:45_

---
