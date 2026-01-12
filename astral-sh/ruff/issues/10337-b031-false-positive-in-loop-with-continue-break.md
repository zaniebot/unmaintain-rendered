```yaml
number: 10337
title: B031 false positive in loop with continue/break/return
type: issue
state: closed
author: MikkelSchubert
labels:
  - bug
  - linter
assignees: []
created_at: 2024-03-11T12:06:00Z
updated_at: 2024-03-25T00:38:32Z
url: https://github.com/astral-sh/ruff/issues/10337
synced_at: 2026-01-12T15:54:50Z
```

# B031 false positive in loop with continue/break/return

---

_@MikkelSchubert_

```python
from __future__ import annotations
from itertools import groupby

def ruff_b031_false_positive(values: list[int]) -> None:
    for key, group in groupby(values):
        if key:
            list(group)
            continue # or break or return

        list(group)
```

```console
$ ruff check --isolated --select B031 testcase.py
testcase.py:10:14: B031 Using the generator returned from `itertools.groupby()` more than once will do nothing on the second usage
Found 1 error.
```

There have been fixes for when the generator is used in different branches of an `if` statements, see for example #4050, but the warning still triggers if you use `continue`, `break`, or `return`. Tested with Ruff version 0.3.2.

---

_Comment by @dhruvmanila on 2024-03-11 13:39_

Thanks! 

I think this can be solved. We would need to consider the block outside the `if` statement as if it's mutually exclusive with the `if` block if there's a `continue` / `break` / `return` as the last statement in the `if` block.

(We so need control flow graphs :))

---

_Label `bug` added by @dhruvmanila on 2024-03-11 13:39_

---

_Label `linter` added by @zanieb on 2024-03-11 16:42_

---

_Comment by @yt2b on 2024-03-23 02:00_

@dhruvmanila 
Can I try to work on this?

---

_Comment by @dhruvmanila on 2024-03-23 16:53_

@yt2b Sure! It might be a bit tricky to get it right because we don't have control flow graph and the implementation uses visitor and stack to simulate it. This [comment](https://github.com/astral-sh/ruff/pull/3844#issuecomment-1495229979) has a visualization on what it looks like. Feel free to ping me with any questions you might have :)

---

_Assigned to @yt2b by @dhruvmanila on 2024-03-23 16:53_

---

_Comment by @dhruvmanila on 2024-03-23 16:53_

(Assigning to you but don't feel any pressure to complete it.)

---

_Closed by @charliermarsh on 2024-03-25 00:38_

---
