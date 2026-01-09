---
number: 1813
title: "Implement isort `reverse_relative` rule"
type: issue
state: closed
author: leroyvn
labels:
  - good first issue
  - isort
assignees: []
created_at: 2023-01-12T10:50:51Z
updated_at: 2023-01-12T20:48:42Z
url: https://github.com/astral-sh/ruff/issues/1813
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement isort `reverse_relative` rule

---

_Issue opened by @leroyvn on 2023-01-12 10:50_

I'd love to move my import sorting to ruff; however I'm missing the "Reverse Relative" rule (see description here: https://pycqa.github.io/isort/docs/configuration/options.html#reverse-relative). Any chance to see it implemented in a foreseeable future?


---

_Label `isort` added by @charliermarsh on 2023-01-12 12:12_

---

_Comment by @charliermarsh on 2023-01-12 15:41_

I inevitably have to read through the source to really understand these options :joy:

---

_Comment by @charliermarsh on 2023-01-12 15:47_

Oh ok, does this just make it such that fewer dots come first? So it considers this correct?

```py
from . import a
from .. import a
from ... import a
```

---

_Comment by @leroyvn on 2023-01-12 15:52_

Yes indeed, it reverses the sorting order of relative module imports and puts the closest to the top.

---

_Comment by @charliermarsh on 2023-01-12 15:55_

Okay, cool, that's easy enough. Can probably go out in the next release later today.

---

_Label `good first issue` added by @charliermarsh on 2023-01-12 15:56_

---

_Comment by @charliermarsh on 2023-01-12 15:56_

Will mark as `good first issue` in case someone wants to knock it out before I get to it!

---

_Referenced in [astral-sh/ruff#1826](../../astral-sh/ruff/pulls/1826.md) on 2023-01-12 18:03_

---

_Comment by @charliermarsh on 2023-01-12 18:08_

I may rename this to `relative-order` and accept `"closest-first"` and `"furthest-first"`. The current setting is a little confusing because it requires you to remember what the default order is (since "reverse" is only relative to the default). Any feedback on that?

---

_Comment by @leroyvn on 2023-01-12 18:37_

I personally do not mind and agree with the terminology you are suggesting, but I think I'm biased.

---

_Closed by @charliermarsh on 2023-01-12 20:48_

---
