```yaml
number: 2423
title: "Add `language: golang` to actionlint pre-commit hook"
type: pull_request
state: merged
author: mschoettle
labels:
  - ci
assignees: []
merged: true
base: main
head: patch-1
created_at: 2026-01-09T19:19:26Z
updated_at: 2026-01-09T19:28:16Z
url: https://github.com/astral-sh/ty/pull/2423
synced_at: 2026-01-10T02:34:11Z
```

# Add `language: golang` to actionlint pre-commit hook

---

_Pull request opened by @mschoettle on 2026-01-09 19:19_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add `language: golang` to the pre-commit hook of actionlint. That way, Renovate will update the shellcheck version in `additional_dependencies` in the future (x-ref https://github.com/astral-sh/ty/pull/2262).

See: https://docs.renovatebot.com/modules/manager/pre-commit/#go


## Test Plan

`uvx prek run --all-files --hook-stage=manual`


---

_Label `ci` added by @AlexWaygood on 2026-01-09 19:23_

---

_Comment by @AlexWaygood on 2026-01-09 19:23_

Thank you very much!

---

_@AlexWaygood approved on 2026-01-09 19:24_

---

_Merged by @AlexWaygood on 2026-01-09 19:26_

---

_Closed by @AlexWaygood on 2026-01-09 19:26_

---

_Branch deleted on 2026-01-09 19:28_

---
