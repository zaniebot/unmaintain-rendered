```yaml
number: 13811
title: "Set `fail_fast: false` in `.pre-commit-config.yaml`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ci
assignees: []
merged: true
base: main
head: pre-commit-ergonomics
created_at: 2024-10-18T12:24:25Z
updated_at: 2024-10-18T15:04:02Z
url: https://github.com/astral-sh/ruff/pull/13811
synced_at: 2026-01-12T15:55:45Z
```

# Set `fail_fast: false` in `.pre-commit-config.yaml`

---

_@AlexWaygood_

## Summary

`fail_fast: true` means that pre-commit stops as soon as a single hook reports a failure. @MichaReiser finds this annoying and so do I: it means that you often have to run `pre-commit run -a` several times in the Ruff repo in order to fix linter complaints in Markdown files.

The default is `fail_fast: false`, but it looks like we overrode the default in https://github.com/astral-sh/ruff/pull/3017.

The docs for the `fail_fast` setting are here: https://pre-commit.com/#top_level-fail_fast

## Test Plan

`pre-commit run -a`


---

_Label `internal` added by @AlexWaygood on 2024-10-18 12:24_

---

_Label `ci` added by @AlexWaygood on 2024-10-18 12:24_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-18 12:24_

---

_@zanieb approved on 2024-10-18 15:03_

---

_Merged by @AlexWaygood on 2024-10-18 15:03_

---

_Closed by @AlexWaygood on 2024-10-18 15:04_

---

_Branch deleted on 2024-10-18 15:04_

---
