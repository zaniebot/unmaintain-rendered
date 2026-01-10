```yaml
number: 11722
title: Fix message when there are no buildable packages
type: pull_request
state: merged
author: damymetzke
labels:
  - error messages
assignees: []
merged: true
base: main
head: fix-message-workspace-no-packages
created_at: 2025-02-23T13:23:42Z
updated_at: 2025-02-24T20:41:14Z
url: https://github.com/astral-sh/uv/pull/11722
synced_at: 2026-01-10T11:10:38Z
```

# Fix message when there are no buildable packages

---

_Pull request opened by @damymetzke on 2025-02-23 13:23_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I noticed that when running "uv build --all-packages" in an empty workspace with no buildable packages, it reports that there are buildable packages. Which I believe to be an error in the message. This patch fixes the typo. I did not find any relevant issues.

## Test Plan

I've verified, to the best of my ability, that this did not introduce any additional errors in related existing tests. Considering the nature of the change I believe it's sufficient.

---

_@charliermarsh approved on 2025-02-24 20:41_

Thanks!

---

_Label `error messages` added by @charliermarsh on 2025-02-24 20:41_

---

_Merged by @charliermarsh on 2025-02-24 20:41_

---

_Closed by @charliermarsh on 2025-02-24 20:41_

---
