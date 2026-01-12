```yaml
number: 5412
title: ruff rule SLOT uses URL to current Python docs
type: pull_request
state: merged
author: cclauss
labels: []
assignees: []
merged: true
base: main
head: ruff-rule-SLOT-uses-url-to-current-Python-docs
created_at: 2023-06-28T09:42:56Z
updated_at: 2023-06-28T09:49:56Z
url: https://github.com/astral-sh/ruff/pull/5412
synced_at: 2026-01-12T15:55:18Z
```

# ruff rule SLOT uses URL to current Python docs

---

_@cclauss_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Currently the URL at the bottom of the `ruff rule SLOT00x` output points to Python 3.7 docs.
Given that Python 3.7 is now end-of-life (as of yesterday), let's instead point users to the current Python docs.

## Test Plan

<!-- How was it tested? -->


---

_@konstin approved on 2023-06-28 09:45_

thanks

---

_Merged by @konstin on 2023-06-28 09:48_

---

_Closed by @konstin on 2023-06-28 09:48_

---

_Branch deleted on 2023-06-28 09:49_

---
