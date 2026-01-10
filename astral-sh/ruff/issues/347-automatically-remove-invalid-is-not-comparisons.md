```yaml
number: 347
title: "Automatically remove invalid `is not` comparisons"
type: issue
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
created_at: 2022-10-07T20:05:20Z
updated_at: 2022-11-07T20:18:05Z
url: https://github.com/astral-sh/ruff/issues/347
synced_at: 2026-01-10T15:56:05Z
```

# Automatically remove invalid `is not` comparisons

---

_Issue opened by @charliermarsh on 2022-10-07 20:05_

From `pyupgrade` (which has so much good stuff):

<img width="792" alt="Screen Shot 2022-10-07 at 4 03 21 PM" src="https://user-images.githubusercontent.com/1309177/194644352-f7f6c2c2-be60-48ce-b41c-c293e15f81a2.png">


---

_Label `rule` added by @charliermarsh on 2022-10-07 20:05_

---

_Comment by @charliermarsh on 2022-10-07 20:27_

Oh, we actually have this already via F632. But we should autofix it.

---

_Closed by @charliermarsh on 2022-11-07 20:18_

---
