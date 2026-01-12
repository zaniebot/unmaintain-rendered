```yaml
number: 10356
title: "docs: github actions example: remove unneeded install step"
type: pull_request
state: merged
author: clintonsteiner
labels:
  - documentation
assignees: []
merged: true
base: main
head: updateGithubActionsExampleToRemoveExtraInstall
created_at: 2025-01-07T12:03:09Z
updated_at: 2025-01-07T13:49:32Z
url: https://github.com/astral-sh/uv/pull/10356
synced_at: 2026-01-12T16:09:15Z
```

# docs: github actions example: remove unneeded install step

---

_@clintonsteiner_

* Previously had uv python install, then uv sync --all-extras --dev
* If we are going to use sync for dev dependencies, then the install step is unneccessary

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Merged by @charliermarsh on 2025-01-07 13:46_

---

_Closed by @charliermarsh on 2025-01-07 13:46_

---

_Comment by @charliermarsh on 2025-01-07 13:46_

Thanks, makes sense.

---

_Label `documentation` added by @charliermarsh on 2025-01-07 13:46_

---

_Branch deleted on 2025-01-07 13:49_

---
