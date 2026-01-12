```yaml
number: 1837
title: Use absolute paths for GitHub and Gitlab annotations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/absolute
created_at: 2023-01-12T22:52:55Z
updated_at: 2023-03-10T16:09:22Z
url: https://github.com/astral-sh/ruff/pull/1837
synced_at: 2026-01-12T04:39:44Z
```

# Use absolute paths for GitHub and Gitlab annotations

---

_Pull request opened by @charliermarsh on 2023-01-12 22:52_

Note that the _annotation path_ is absolute, while the path encoded in the message remains relative.

![Screen Shot 2023-01-12 at 5 54 11 PM](https://user-images.githubusercontent.com/1309177/212198531-63f15445-0f6a-471c-a64c-18ad2b6df0c7.png)

Closes #1835.


---

_Merged by @charliermarsh on 2023-01-12 22:54_

---

_Closed by @charliermarsh on 2023-01-12 22:54_

---

_Branch deleted on 2023-01-12 22:54_

---

_Comment by @Agalin on 2023-03-10 15:56_

What was the reason for this change? At least for Gitlab [`path` should be relative](https://docs.gitlab.com/ee/ci/testing/code_quality.html#implement-a-custom-tool) to the `CI_PROJECT_DIR` location. With this implementation links in widgets are broken.

---

_Comment by @charliermarsh on 2023-03-10 16:01_

Check out the linked issue: #1835. Sounds like we may need to change GitLab paths to be relative.

---

_Comment by @Agalin on 2023-03-10 16:09_

Ah. missed the issue link. 😳 
Will continue in the new one, thanks!

---
