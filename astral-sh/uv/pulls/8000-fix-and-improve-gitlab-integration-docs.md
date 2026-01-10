```yaml
number: 8000
title: Fix and improve GitLab integration docs
type: pull_request
state: merged
author: sisp
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/gitlab
created_at: 2024-10-08T10:51:57Z
updated_at: 2024-10-08T14:49:57Z
url: https://github.com/astral-sh/uv/pull/8000
synced_at: 2026-01-10T12:54:01Z
```

# Fix and improve GitLab integration docs

---

_Pull request opened by @sisp on 2024-10-08 10:51_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I've fixed and improved the GitLab integration docs:

- Some syntax fixes (especially use of a _relative_ path for the cache directory as required according to [GitLab CI cache path](https://docs.gitlab.com/ee/ci/yaml/#cachepaths) docs)
- Some more idiomatic syntax and naming

I've split the changes across several commits, intended to be rebase-merged and not squashed, to avoid mixing concerns.

## Test Plan

I've created a demo project: https://gitlab.com/sisp/gitlab-uv-demo


---

_Renamed from "Docs/gitlab" to "Fix and improve GitLab integration docs" by @sisp on 2024-10-08 10:56_

---

_Label `documentation` added by @zanieb on 2024-10-08 14:47_

---

_Comment by @zanieb on 2024-10-08 14:47_

Thanks!

---

_@zanieb approved on 2024-10-08 14:47_

---

_Merged by @zanieb on 2024-10-08 14:48_

---

_Closed by @zanieb on 2024-10-08 14:48_

---

_Branch deleted on 2024-10-08 14:49_

---
