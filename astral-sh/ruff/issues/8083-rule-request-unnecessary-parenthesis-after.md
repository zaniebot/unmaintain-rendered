```yaml
number: 8083
title: "Rule request: unnecessary parenthesis after `enumerate`"
type: issue
state: closed
author: jamesbraza
labels:
  - needs-decision
assignees: []
created_at: 2023-10-20T01:46:51Z
updated_at: 2025-08-11T07:36:31Z
url: https://github.com/astral-sh/ruff/issues/8083
synced_at: 2026-01-12T15:54:47Z
```

# Rule request: unnecessary parenthesis after `enumerate`

---

_@jamesbraza_

With `ruff==0.1.1`:

```python
for [_i, _hi] in enumerate(range(5)):
    pass
for (_i, _hi) in enumerate(range(5)):
    pass
```

It would be great to have an autofix to remove the extra parenthesis/bracket wrapping.

Testing `pylint==3.0.1` and `refurb==1.22.0`, it doesn't seem either of these tools catch this possible improvement either.

---

_Comment by @charliermarsh on 2023-10-20 21:22_

Are you referring here to the parentheses around `(_i, _hi)`?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-10-20 21:22_

---

_Comment by @jamesbraza on 2023-10-20 21:38_

Yeah the parenthesis or brackets around `i, hi`.  Sorry for the unclear OP!

An aside is the example was supposed to be like a secret high five

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-10-23 03:55_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-23 03:55_

---

_Comment by @MichaReiser on 2025-08-11 07:36_

I'll merge this into https://github.com/astral-sh/ruff/issues/6906 While this issue is slightly more specific, the general ask seems to be the same --- removing unnecessary parentheses

---

_Closed by @MichaReiser on 2025-08-11 07:36_

---
