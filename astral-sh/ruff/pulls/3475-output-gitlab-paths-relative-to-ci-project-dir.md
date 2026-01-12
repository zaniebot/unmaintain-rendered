```yaml
number: 3475
title: "Output GitLab paths relative to `CI_PROJECT_DIR`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/gitlab
created_at: 2023-03-13T02:55:38Z
updated_at: 2023-03-13T03:09:10Z
url: https://github.com/astral-sh/ruff/pull/3475
synced_at: 2026-01-12T15:55:12Z
```

# Output GitLab paths relative to `CI_PROJECT_DIR`

---

_@charliermarsh_

It turns out that GitLab [expects relative paths](https://docs.gitlab.com/ee/ci/testing/code_quality.html#implement-a-custom-tool) in its structured output. I can't find a definitive link to back up the claim that they're meant to be relative to `CI_PROJECT_DIR`, but I see it in some StackOverflow answers, and you see it in some other [GitLab docs](https://docs.gitlab.com/13.12/ee/ci/yaml/README.html#artifactspaths).

Closes #3443.


---

_Merged by @charliermarsh on 2023-03-13 03:03_

---

_Closed by @charliermarsh on 2023-03-13 03:03_

---

_Branch deleted on 2023-03-13 03:03_

---

_Comment by @github-actions[bot] on 2023-03-13 03:09_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---
