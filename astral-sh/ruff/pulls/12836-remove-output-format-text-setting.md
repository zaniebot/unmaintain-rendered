```yaml
number: 12836
title: "Remove `output-format=text` setting"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - configuration
assignees: []
merged: true
base: ruff-0.7
head: remove-output-format-text
created_at: 2024-08-12T09:19:36Z
updated_at: 2024-10-08T11:47:15Z
url: https://github.com/astral-sh/ruff/pull/12836
synced_at: 2026-01-10T20:59:36Z
```

# Remove `output-format=text` setting

---

_Pull request opened by @MichaReiser on 2024-08-12 09:19_

## Summary

Using `output-format=text` is a hard error since Ruff 0.5 (https://github.com/astral-sh/ruff/pull/12010). 

This PR removes the custom error message.

## Test Plan

```
ruff failed
  Cause: Failed to parse /home/micha/astral/test/pyproject.toml
  Cause: TOML parse error at line 4, column 15
  |
4 | output-format="text"
  |               ^^^^^^
unknown variant `text`, expected one of `concise`, `full`, `json`, `json-lines`, `junit`, `grouped`, `github`, `gitlab`, `pylint`, `rdjson`, `azure`, `sarif`
```

---

_Label `configuration` added by @MichaReiser on 2024-08-12 09:20_

---

_Comment by @github-actions[bot] on 2024-08-12 11:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-08-12 11:59_

Same thoughts as https://github.com/astral-sh/ruff/pull/12833#pullrequestreview-2232732050 -- I'd probably keep the nice error message for another minor version personally, but don't have a strong opinion

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-12 14:00_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-08 11:33_

---

_@AlexWaygood approved on 2024-10-08 11:35_

Thanks!

---

_Merged by @MichaReiser on 2024-10-08 11:46_

---

_Closed by @MichaReiser on 2024-10-08 11:46_

---

_Branch deleted on 2024-10-08 11:46_

---

_Label `internal` added by @AlexWaygood on 2024-10-08 11:47_

---
