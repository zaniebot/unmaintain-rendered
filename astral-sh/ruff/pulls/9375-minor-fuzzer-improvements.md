```yaml
number: 9375
title: Minor fuzzer improvements
type: pull_request
state: merged
author: addisoncrump
labels:
  - fuzzer
assignees: []
merged: true
base: main
head: minor-fuzzer-fixes
created_at: 2024-01-03T01:46:31Z
updated_at: 2024-01-03T01:52:42Z
url: https://github.com/astral-sh/ruff/pull/9375
synced_at: 2026-01-10T23:07:18Z
```

# Minor fuzzer improvements

---

_Pull request opened by @addisoncrump on 2024-01-03 01:46_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Adds timeouts to fuzzer cmin stages in the case of an infinite loop
- Adds executable flag to reinit-fuzzer.sh because it was annoying me

## Test Plan

Not needed.


---

_@charliermarsh approved on 2024-01-03 01:50_

---

_Label `fuzzer` added by @charliermarsh on 2024-01-03 01:50_

---

_Merged by @charliermarsh on 2024-01-03 01:52_

---

_Closed by @charliermarsh on 2024-01-03 01:52_

---
