```yaml
number: 5066
title: Ruff nested min max PYLW3301 autofix bug?
type: issue
state: closed
author: Ryang20718
labels:
  - question
assignees: []
created_at: 2023-06-13T22:24:17Z
updated_at: 2023-06-13T23:19:17Z
url: https://github.com/astral-sh/ruff/issues/5066
synced_at: 2026-01-10T11:09:47Z
```

# Ruff nested min max PYLW3301 autofix bug?

---

_Issue opened by @Ryang20718 on 2023-06-13 22:24_

ruff version 0.0.272

```
print(min(1, 2, 3, min(4)))
```

With pylint warning enabled, ruff check --fix on this file causes autofix to return
```
print(min(1, 2, 3, *4))
```

---

_Comment by @charliermarsh on 2023-06-13 22:53_

Hmm, but `min(4)` is not valid either? If you provide a single argument to `min`, I believe it has to be iterable. The above gives: `TypeError: 'int' object is not iterable`.

---

_Label `question` added by @charliermarsh on 2023-06-13 22:53_

---

_Comment by @Ryang20718 on 2023-06-13 23:12_

ah you're right, I always ran ruff before actually running the code. 😭 I'm just being dumb 

---

_Closed by @Ryang20718 on 2023-06-13 23:12_

---

_Comment by @charliermarsh on 2023-06-13 23:19_

No worries, I had to check it myself too :joy:

---
