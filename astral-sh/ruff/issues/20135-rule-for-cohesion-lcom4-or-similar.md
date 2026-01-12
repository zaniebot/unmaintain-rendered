```yaml
number: 20135
title: Rule for cohesion (LCOM4 or similar)
type: issue
state: open
author: scastlara
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-28T13:10:36Z
updated_at: 2025-09-06T08:25:58Z
url: https://github.com/astral-sh/ruff/issues/20135
synced_at: 2026-01-12T15:54:57Z
```

# Rule for cohesion (LCOM4 or similar)

---

_@scastlara_

### Summary

Hi! 👋 

I was looking at rules that measure cohesion in ruff, and I did not find much.

There is this library https://github.com/mschwager/cohesion, which can work as a flake8 plugin, with a rule (`H601 class has low cohesion`).

Any plans to implement something similar? Or maybe something like this already exists in ruff and I missed it? 

Thanks!

---

_Comment by @ntBre on 2025-08-28 13:55_

Ah interesting, I found some related discussion on this issue around flake8-cognitive-complexity: https://github.com/astral-sh/ruff/issues/2418.

We don't have any current plans to implement something like this, but we can certainly track interest here!

---

_Label `rule` added by @ntBre on 2025-08-28 13:55_

---

_Label `needs-decision` added by @ntBre on 2025-08-28 13:55_

---

_Comment by @second-ed on 2025-09-06 08:25_

I'd recommend using a [slightly more modern metric](https://ieeexplore.ieee.org/document/8351747) than cohesion to measure this. I've done some work on this sort of thing before and it's definitely well within `ruff`s capability by decomposing a directory into a call-graph, but I admit it's quite a niche topic when compared to more common linting rules. 




---
