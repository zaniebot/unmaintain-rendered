```yaml
number: 10243
title: "docs: Explicitly specify to add a new repo entry to the repos list item in the `.pre-commit-config.yaml`"
type: pull_request
state: merged
author: eva-mueller-coremedia
labels:
  - documentation
assignees: []
merged: true
base: main
head: pre-commit-docs
created_at: 2024-12-30T19:15:11Z
updated_at: 2025-05-27T16:25:23Z
url: https://github.com/astral-sh/uv/pull/10243
synced_at: 2026-01-12T16:09:11Z
```

# docs: Explicitly specify to add a new repo entry to the repos list item in the `.pre-commit-config.yaml`

---

_@eva-mueller-coremedia_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When creating the `.pre-commit-config.yaml` from scratch, although following https://pre-commit.com/, it might be easy to overlook that the pre-commit repo examples need to be added below the `repos` list item to get a valid `yaml` file.

Additionally, updated the version of the first two examples.

## Test Plan

I followed the `CONTRIBUTING.md` and the result looked fine.


---

_Review requested from @zanieb by @konstin on 2025-04-28 12:57_

---

_Assigned to @zanieb by @zanieb on 2025-04-28 13:43_

---

_Label `documentation` added by @zanieb on 2025-04-28 13:44_

---

_@zanieb approved on 2025-05-27 16:24_

---

_Merged by @zanieb on 2025-05-27 16:25_

---

_Closed by @zanieb on 2025-05-27 16:25_

---
