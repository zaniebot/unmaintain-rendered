```yaml
number: 7034
title: "Add dependabot for `cargo` dependencies"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot
created_at: 2023-08-31T20:24:09Z
updated_at: 2023-09-01T14:47:41Z
url: https://github.com/astral-sh/ruff/pull/7034
synced_at: 2026-01-12T15:55:23Z
```

# Add dependabot for `cargo` dependencies

---

_@zanieb_

Ideally we shouldn't have to run `cargo update` manually — it requires us to remember to do so and groups all updates into a single pull request making it challenging to determine which upgrade introduces regressions e.g. #6964. Here we add daily checks for cargo dependency updates.

This pull request also simplifies dependabot configuration for GitHub Actions versions.




---

_Label `internal` added by @zanieb on 2023-08-31 20:24_

---

_@MichaReiser approved on 2023-09-01 06:26_

We can give it a try. Rolling back is easy. I always found dependabot a pain when working on JS projects. 

---

_Comment by @charliermarsh on 2023-09-01 13:57_

I'm open to trying it.

One request: when we merge Dependabot PRs, can we standardize on manually removing the PR body, to avoid these excessively long Git logs?

<img width="868" alt="Screen Shot 2023-09-01 at 2 56 42 PM" src="https://github.com/astral-sh/ruff/assets/1309177/7428bf19-4b0b-4b68-8868-4a3179e59f0f">


---

_Comment by @zanieb on 2023-09-01 14:47_

@charliermarsh should we change the default squash commit message (in general) to just be pull request titles?

---

_Merged by @zanieb on 2023-09-01 14:47_

---

_Closed by @zanieb on 2023-09-01 14:47_

---

_Branch deleted on 2023-09-01 14:47_

---
