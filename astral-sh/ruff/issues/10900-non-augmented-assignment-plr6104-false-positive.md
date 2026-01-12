```yaml
number: 10900
title: "`non-augmented-assignment` (`PLR6104`) - false positive on operators where the variable is on the other side"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - help wanted
  - preview
assignees: []
created_at: 2024-04-12T07:21:03Z
updated_at: 2024-04-12T14:56:42Z
url: https://github.com/astral-sh/ruff/issues/10900
synced_at: 2026-01-12T15:54:50Z
```

# `non-augmented-assignment` (`PLR6104`) - false positive on operators where the variable is on the other side

---

_@DetachHead_

```py
a = 1
a = 0 / a  # PLR6104: Use `/=` to perform an augmented assignment directly
```
[playground](https://play.ruff.rs/9874300d-a8c6-4e83-a3b9-841935687a53)

in this example, the quickfix converts it to `a /= 0` which causes a runtime `ZeroDivisionError`

---

_Label `bug` added by @MichaReiser on 2024-04-12 07:29_

---

_Label `preview` added by @MichaReiser on 2024-04-12 07:29_

---

_Comment by @MichaReiser on 2024-04-12 07:29_

CC: @lshi18 

---

_Label `help wanted` added by @MichaReiser on 2024-04-12 07:29_

---

_Renamed from "`non-augmented-assignment` (`PLR6104`) - false positive on operatrors where the variable is on the other side" to "`non-augmented-assignment` (`PLR6104`) - false positive on operators where the variable is on the other side" by @DetachHead on 2024-04-12 07:30_

---

_Comment by @lshi18 on 2024-04-12 11:57_

Hi @MichaReiser, I think that [this implementation](https://github.com/astral-sh/ruff/pull/9932/files#diff-9627d6862661b7c0b6d946bfeb654fdf89bbe3f78d272be2c4df23964da961c7R107) is only correct for commutative binary operators, but fails for others such as "/". 

And thank @charliermarsh for completing the work in #9932 . 

---

_Comment by @charliermarsh on 2024-04-12 13:01_

Ah thanks, I’ll fix that today!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-12 13:47_

---

_Closed by @charliermarsh on 2024-04-12 14:04_

---

_Comment by @NeilGirdhar on 2024-04-12 14:54_

@charliermarsh Is this fixed for:
```python
test_string = "desired = " + test_string  # False positive PLR6104
```
since `+` is sometimes commutative.

---

_Comment by @charliermarsh on 2024-04-12 14:56_

No, I'll fix that separately.

---
