```yaml
number: 22483
title: "Add language: golang to actionlint pre-commit hook"
type: pull_request
state: merged
author: mschoettle
labels:
  - ci
assignees: []
merged: true
base: main
head: patch-1
created_at: 2026-01-09T19:21:17Z
updated_at: 2026-01-09T19:53:33Z
url: https://github.com/astral-sh/ruff/pull/22483
synced_at: 2026-01-10T15:56:07Z
```

# Add language: golang to actionlint pre-commit hook

---

_Pull request opened by @mschoettle on 2026-01-09 19:21_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add `language: golang` to the pre-commit hook of actionlint. That way, Renovate will update the shellcheck version in `additional_dependencies` in the future (x-ref https://github.com/astral-sh/ruff/pull/22285).

See: https://docs.renovatebot.com/modules/manager/pre-commit/#go


## Test Plan

`uvx prek run --all-files --hook-stage=manual`


---

_@AlexWaygood approved on 2026-01-09 19:25_

---

_Label `ci` added by @AlexWaygood on 2026-01-09 19:27_

---

_Merged by @AlexWaygood on 2026-01-09 19:30_

---

_Closed by @AlexWaygood on 2026-01-09 19:30_

---

_Branch deleted on 2026-01-09 19:53_

---
