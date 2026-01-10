```yaml
number: 7912
title: "Add `UV_FIND_LINKS` environment variable support for the `--find-links` command-line option"
type: pull_request
state: merged
author: SethMMorton
labels:
  - configuration
assignees: []
merged: true
base: main
head: main
created_at: 2024-10-04T03:27:31Z
updated_at: 2024-10-04T11:30:49Z
url: https://github.com/astral-sh/uv/pull/7912
synced_at: 2026-01-10T12:53:59Z
```

# Add `UV_FIND_LINKS` environment variable support for the `--find-links` command-line option

---

_Pull request opened by @SethMMorton on 2024-10-04 03:27_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds support for the `UV_FIND_LINKS` environment variable as an alternative to the `--find-links` command-line option, as requested in #1839.

## Test Plan

A unit test was added to validate that setting `UV_FIND_LINKS` provided the same result as a link provided with the `--find-links` command-line option.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-04 11:15_

---

_Label `configuration` added by @charliermarsh on 2024-10-04 11:16_

---

_@charliermarsh approved on 2024-10-04 11:24_

Thanks!

---

_Renamed from "Add UV_FIND_LINKS environment variable support for the --find-links command-line option" to "Add `UV_FIND_LINKS` environment variable support for the `--find-links` command-line option" by @charliermarsh on 2024-10-04 11:24_

---

_Merged by @charliermarsh on 2024-10-04 11:30_

---

_Closed by @charliermarsh on 2024-10-04 11:30_

---
