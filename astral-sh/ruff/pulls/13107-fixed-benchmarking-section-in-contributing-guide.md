```yaml
number: 13107
title: Fixed benchmarking section in Contributing guide
type: pull_request
state: merged
author: teofr
labels:
  - documentation
assignees: []
merged: true
base: main
head: teofr/fix_contribution_benchmarking
created_at: 2024-08-26T11:43:35Z
updated_at: 2024-08-26T13:40:50Z
url: https://github.com/astral-sh/ruff/pull/13107
synced_at: 2026-01-10T21:38:32Z
```

# Fixed benchmarking section in Contributing guide

---

_Pull request opened by @teofr on 2024-08-26 11:43_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Noticed there was a wrong tip on the Contributing guide, `cargo benchmark lexer` wouldn't run any benches.
Probably a missed update on #9535 

It may make sense to remove the `cargo benchmark` command from the guide altogether, but up to the mantainers.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

No tests

<!-- How was it tested? -->


---

_@dhruvmanila approved on 2024-08-26 13:18_

Thanks

---

_Label `documentation` added by @dhruvmanila on 2024-08-26 13:18_

---

_Merged by @dhruvmanila on 2024-08-26 13:19_

---

_Closed by @dhruvmanila on 2024-08-26 13:19_

---

_Branch deleted on 2024-08-26 13:40_

---
