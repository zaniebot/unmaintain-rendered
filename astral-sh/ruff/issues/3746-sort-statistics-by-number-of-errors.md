```yaml
number: 3746
title: Sort statistics by number of errors
type: issue
state: closed
author: JonathanPlasse
labels:
  - wish
assignees: []
created_at: 2023-03-26T18:34:43Z
updated_at: 2023-03-26T20:45:37Z
url: https://github.com/astral-sh/ruff/issues/3746
synced_at: 2026-01-12T15:54:44Z
```

# Sort statistics by number of errors

---

_@JonathanPlasse_

IMO, `ruff check --statistics --select=ALL .` should be sorted from the highest to lowest number of errors.
On a new codebase, it would help to know which rules to prioritize.

---

_Comment by @charliermarsh on 2023-03-26 19:09_

Seems reasonable, I actually thought this was how it worked already but I guess not!

---

_Label `wishlist` added by @charliermarsh on 2023-03-26 19:10_

---

_Comment by @JonathanPlasse on 2023-03-26 20:18_

I will work on it.

---

_Assigned to @JonathanPlasse by @charliermarsh on 2023-03-26 20:24_

---

_Closed by @charliermarsh on 2023-03-26 20:45_

---
