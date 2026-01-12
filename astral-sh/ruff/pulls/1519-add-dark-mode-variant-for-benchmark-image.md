```yaml
number: 1519
title: Add dark mode variant for benchmark image
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/dark
created_at: 2022-12-31T22:44:59Z
updated_at: 2023-01-28T16:48:51Z
url: https://github.com/astral-sh/ruff/pull/1519
synced_at: 2026-01-12T04:51:59Z
```

# Add dark mode variant for benchmark image

---

_Pull request opened by @charliermarsh on 2022-12-31 22:44_

Resolves: #1515.

---

_Merged by @charliermarsh on 2022-12-31 22:47_

---

_Closed by @charliermarsh on 2022-12-31 22:47_

---

_Branch deleted on 2022-12-31 22:47_

---

_Comment by @not-my-profile on 2023-01-28 07:09_

It appears that PyPI doesn't support the `<picture>` tag? https://pypi.org/project/ruff/:

![image](https://user-images.githubusercontent.com/73739153/215252377-da532952-a157-4d94-abbe-ada70ccdf5d4.png)


---

_Comment by @charliermarsh on 2023-01-28 12:51_

Yeah this should be fixed in the next Ruff release.

---

_Comment by @charliermarsh on 2023-01-28 16:48_

Lol, ok, it got better, but still off:

<img width="873" alt="Screen Shot 2023-01-28 at 11 48 28 AM" src="https://user-images.githubusercontent.com/1309177/215278622-d3303877-fd1f-4d6b-9687-d5e9ad19afd0.png">


---
