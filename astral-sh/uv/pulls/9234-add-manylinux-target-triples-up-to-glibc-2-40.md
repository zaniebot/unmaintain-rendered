```yaml
number: 9234
title: Add manylinux target triples up to glibc 2.40
type: pull_request
state: merged
author: filaretov
labels: []
assignees: []
merged: true
base: main
head: add-more-manylinux-platform-tags
created_at: 2024-11-19T16:49:44Z
updated_at: 2024-11-19T19:37:43Z
url: https://github.com/astral-sh/uv/pull/9234
synced_at: 2026-01-12T16:08:43Z
```

# Add manylinux target triples up to glibc 2.40

---

_@filaretov_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

PR #4965 added `*-manylinux_2_31` as a target triple, and issue #4966 described the need for a more general solution.

In lieu of a general solution, this PR adds further explicit manylinux target triples for different glibc version up to the one used by the latest Ubuntu release (glibc 2.40 used in Ubuntu 24.10).

## Test Plan

<!-- How was it tested? -->

Local, manual testing with a Python wheel targeting `x86_64-manylinux_2_35`.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-19 16:50_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-11-19 16:50_

---

_@charliermarsh approved on 2024-11-19 19:37_

---

_Merged by @charliermarsh on 2024-11-19 19:37_

---

_Closed by @charliermarsh on 2024-11-19 19:37_

---
