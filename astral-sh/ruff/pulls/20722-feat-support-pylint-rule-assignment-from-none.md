```yaml
number: 20722
title: "feat: support pylint rule assignment-from-none"
type: pull_request
state: closed
author: fatelei
labels: []
assignees: []
base: main
head: assignment-from-none
created_at: 2025-10-06T14:47:28Z
updated_at: 2025-10-06T14:50:44Z
url: https://github.com/astral-sh/ruff/pull/20722
synced_at: 2026-01-12T15:57:08Z
```

# feat: support pylint rule assignment-from-none

---

_@fatelei_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

feat: support pylint rule assignment-from-none

## Test Plan

related unittest


---

_@MichaReiser reviewed on 2025-10-06 14:49_

Thank you for working on this. 

I don't think we should add this rule, given that Ruff can only resolve function calls in the same file. The false-negative rate is just too high (without multifile analysis)

---

_Closed by @fatelei on 2025-10-06 14:50_

---
