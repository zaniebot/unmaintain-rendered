```yaml
number: 12451
title: "F821: false positive if variable is defined in a loop"
type: issue
state: open
author: JaRoSchm
labels:
  - type-inference
assignees: []
created_at: 2024-07-22T11:29:30Z
updated_at: 2024-07-23T07:13:11Z
url: https://github.com/astral-sh/ruff/issues/12451
synced_at: 2026-01-12T15:54:51Z
```

# F821: false positive if variable is defined in a loop

---

_@JaRoSchm_

Hi,

this examples raises F821 in ruff 0.5.4 although it clearly is working:

```python
for i in range(3):
    if i == 0:
        x = 0
    else:
        x = y

    y = x + 2
```

This happens if the variable is defined inside of the loop body.
Searching through the many issues where F821 is mentioned I couldn't find this issue yet.
Output of `ruff check --select F821 --preview`:

```
test.py:5:13: F821 Undefined name `y`
  |
3 |         x = 0
4 |     else:
5 |         x = y
  |             ^ F821
6 | 
7 |     y = x + 2
  |

Found 1 error.
```

---

_Label `type-inference` added by @MichaReiser on 2024-07-23 07:11_

---

_Comment by @MichaReiser on 2024-07-23 07:13_

I think this is hard for ruff to get right at the moment without type inference because Ruff can't reason about that `i` starts with `0` and that the first branch is guaranteed to be taken because of that before ever taking the other branch. 

---
