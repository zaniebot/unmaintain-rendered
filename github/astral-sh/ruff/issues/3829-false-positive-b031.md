---
number: 3829
title: "False positive: B031"
type: issue
state: closed
author: AA-Turner
labels:
  - bug
assignees: []
created_at: 2023-03-31T16:16:07Z
updated_at: 2023-03-31T17:11:59Z
url: https://github.com/astral-sh/ruff/issues/3829
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive: B031

---

_Issue opened by @AA-Turner on 2023-03-31 16:16_

Ruff 0.0.260

Reproducer:

```pwsh
(sphinx) PS I:\Development\sphinx> ruff -V                                                
ruff 0.0.260
(sphinx) PS I:\Development\sphinx> type bug_b031.py
from itertools import groupby


def f():
    for w, g in groupby('string', len):
        if w == 1:
            return (''.join(g)).split(" ")
        else:
            return list(g)  # Ruff indicates B031 here


def g():
    for w, g in groupby('string', len):
        tmp = [*g]  # by introducing an interstitial variable, we avoid B031
        if w == 1:
            return (''.join(tmp)).split(" ")
        else:
            return list(tmp)
(sphinx) PS I:\Development\sphinx> ruff --isolated --show-source --select B031 bug_b031.py
bug_b031.py:9:25: B031 Using the generator returned from `itertools.groupby()` more than once will do nothing on the second usage
  |
9 |             return list(g)  # Ruff indicates B031 here
  |                         ^ B031
  |

Found 1 error.
(sphinx) PS I:\Development\sphinx> 
```

<details>
<summary> <code>bug_b031.py</code> </summary>

```python
from itertools import groupby


def f():
    for w, g in groupby('string', len):
        if w == 1:
            return (''.join(g)).split(" ")
        else:
            return list(g)  # Ruff indicates B031 here


def g():
    for w, g in groupby('string', len):
        tmp = [*g]  # by introducing an interstitial variable, we avoid B031
        if w == 1:
            return (''.join(tmp)).split(" ")
        else:
            return list(tmp)
```

</details>

Sorry for the rather contrived example, this was reduced from some code in Sphinx. Broadly I think that the error comes from the heuristic checking *any* re-use, whereas here as the symbol `g` is used twice, but only once per branch of an if/elif/else chain. 

Please would it be possible to add some heuristic for tracking usages like this, in if/elif/else chains?

Thanks,
Adam

---

_Comment by @charliermarsh on 2023-03-31 16:22_

👍 I think this is the same as #3801. `flake8-bugbear` also has this problem. I think we should be able to avoid it though.

---

_Label `bug` added by @charliermarsh on 2023-03-31 16:22_

---

_Comment by @AA-Turner on 2023-03-31 16:23_

Ahh, sorry--should have had a quick look before submitting this!

Thanks,
Adam

---

_Comment by @charliermarsh on 2023-03-31 16:24_

All good, it's useful to know that others have hit this too and to see another example!

---

_Comment by @dhruvmanila on 2023-03-31 16:55_

I'll try to look into it over the weekend but I guess it would be better to track it in one issue. We could move the example in the other one.

---

_Comment by @charliermarsh on 2023-03-31 17:11_

@dhruvmanila - Sounds good, I'm going to mark as a duplicate.

---

_Closed by @charliermarsh on 2023-03-31 17:11_

---
