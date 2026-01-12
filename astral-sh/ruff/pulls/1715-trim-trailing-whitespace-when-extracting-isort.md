```yaml
number: 1715
title: Trim trailing whitespace when extracting isort directives
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: trim-end-when-extracting-isort-directives
created_at: 2023-01-07T16:35:22Z
updated_at: 2023-01-08T01:08:03Z
url: https://github.com/astral-sh/ruff/pull/1715
synced_at: 2026-01-12T05:36:32Z
```

# Trim trailing whitespace when extracting isort directives

---

_Pull request opened by @harupy on 2023-01-07 16:35_

_No description provided._

---

_@harupy reviewed on 2023-01-07 16:36_

---

_Review comment by @harupy on `resources/test/fixtures/isort/split.py`:9 on 2023-01-07 16:36_

This line contains two trailing spaces

---

_@charliermarsh reviewed on 2023-01-07 17:19_

---

_Review comment by @charliermarsh on `src/directives.rs`:122 on 2023-01-07 17:19_

What about comments with multiple directives?

![Screen Shot 2023-01-07 at 12 18 50 PM](https://user-images.githubusercontent.com/1309177/211162733-4e1e34dc-fe0e-48a6-9554-a9b827b2c665.png)


---

_Review comment by @harupy on `src/directives.rs`:122 on 2023-01-07 17:34_

I see, then Let's revert this change.

---

_@harupy reviewed on 2023-01-07 17:34_

---

_Merged by @charliermarsh on 2023-01-07 17:39_

---

_Closed by @charliermarsh on 2023-01-07 17:39_

---

_Branch deleted on 2023-01-08 01:08_

---
