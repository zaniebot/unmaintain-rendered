```yaml
number: 7375
title: Show rule codes in shell tab completion
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/possible-values
created_at: 2023-09-14T01:15:30Z
updated_at: 2023-09-14T18:45:53Z
url: https://github.com/astral-sh/ruff/pull/7375
synced_at: 2026-01-12T02:39:09Z
```

# Show rule codes in shell tab completion

---

_Pull request opened by @charliermarsh on 2023-09-14 01:15_

## Summary

I noticed that we have a custom parser for rule selectors, but it wasn't actually being used? This PR adds it back to our Clap setup and changes the parser to only show full categories and individual rules when tab-completing:

<img width="1792" alt="Screen Shot 2023-09-13 at 9 13 38 PM" src="https://github.com/astral-sh/ruff/assets/1309177/028b18d2-8c92-49c1-b781-f24c9ae310f7">

<img width="1792" alt="Screen Shot 2023-09-13 at 9 13 40 PM" src="https://github.com/astral-sh/ruff/assets/1309177/fd598da5-78fb-412d-a69e-2a0963d479cd">

<img width="1792" alt="Screen Shot 2023-09-13 at 9 13 58 PM" src="https://github.com/astral-sh/ruff/assets/1309177/7c482b90-6e54-425c-ae23-fb50496a177a">

The previous implementation showed all codes, which I found too noisy:

<img width="1792" alt="Screen Shot 2023-09-13 at 8 57 09 PM" src="https://github.com/astral-sh/ruff/assets/1309177/db370a0e-2a9f-4acd-b1e3-224a1f8e9ce5">


---

_Label `cli` added by @charliermarsh on 2023-09-14 01:15_

---

_Review requested from @zanieb by @charliermarsh on 2023-09-14 01:15_

---

_Comment by @github-actions[bot] on 2023-09-14 03:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@dhruvmanila approved on 2023-09-14 03:23_

---

_Merged by @charliermarsh on 2023-09-14 18:37_

---

_Closed by @charliermarsh on 2023-09-14 18:37_

---

_Branch deleted on 2023-09-14 18:37_

---
