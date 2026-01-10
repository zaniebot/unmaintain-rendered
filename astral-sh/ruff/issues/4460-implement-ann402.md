```yaml
number: 4460
title: Implement ANN402
type: issue
state: open
author: janosh
labels:
  - rule
assignees: []
created_at: 2023-05-16T22:01:19Z
updated_at: 2025-03-20T08:54:44Z
url: https://github.com/astral-sh/ruff/issues/4460
synced_at: 2026-01-10T11:09:47Z
```

# Implement ANN402

---

_Issue opened by @janosh on 2023-05-16 22:01_

Flag and auto-fix [`flake8-annotations`](https://github.com/sco1/flake8-annotations#opinionated-warnings) `ANN402` type comments.

```diff
- nums = [] # type: List[float]
+ nums: list[float] = []  # if target=py3.10+ or future annotations imported

+ from typing import List
+ nums: List[float] = []  # else
```

---

_Comment by @JonathanPlasse on 2023-05-17 06:11_

It is already flagged by `PYI033`.
Only the auto-fix remain to be implemented.

---

_Comment by @janosh on 2023-05-17 13:07_

Could we add an alias from ANN402 to PYI033? or a link from one to the other in the docs?

---

_Comment by @janosh on 2023-05-17 15:54_

@JonathanPlasse I just had a look at [the docs](https://beta.ruff.rs/docs/rules/#flake8-pyi-pyi). Actually `PYI033` only applies to stub files. It doesn't flag type comments in regular python files.

---

_Comment by @calumy on 2023-05-17 20:43_

Red #4166 which makes a start on enabling this rule for .py files. 

---

_Label `rule` added by @charliermarsh on 2023-05-18 00:50_

---

_Comment by @p1-dta on 2025-03-20 08:54_

Hi, just noticed the 402 were missing from flake8-annotations, is there any plans to implement this in the near future?

---
