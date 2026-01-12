```yaml
number: 6770
title: Update formatter ecosystem checks revisions
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: update-formatter-ecosystem-check-hashes
created_at: 2023-08-22T12:24:32Z
updated_at: 2023-08-22T18:19:11Z
url: https://github.com/astral-sh/ruff/pull/6770
synced_at: 2026-01-12T15:55:22Z
```

# Update formatter ecosystem checks revisions

---

_@konstin_

With https://github.com/django/django/pull/17181 merged, this removes an odd edge case (tuple expression statements aka bogus trailing commas after statements that turn them into a tuple without you noticing) that we don't want to care about because the input code is ~wrong from the similarity index. I've took this opportunity to update the revisions of all projects we test.

main

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.75477          |
| django       | 0.99814          |
| transformers | 0.99621          |
| twine        | 0.99876          |
| typeshed     | 0.99953          |
| warehouse    | 0.99601          |
| zulip        | 0.99727          |

this PR

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.75996          |
| django       | 0.99819          |
| transformers | 0.99622          |
| twine        | 0.99876          |
| typeshed     | 0.99953          |
| warehouse    | 0.99607          |
| zulip        | 0.99729          |



---

_Label `formatter` added by @konstin on 2023-08-22 12:24_

---

_Comment by @github-actions[bot] on 2023-08-22 12:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-08-22 12:48_

---

_Merged by @charliermarsh on 2023-08-22 18:19_

---

_Closed by @charliermarsh on 2023-08-22 18:19_

---

_Branch deleted on 2023-08-22 18:19_

---
