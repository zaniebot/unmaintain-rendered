```yaml
number: 14428
title: "Remove the deprecated `E999` rule code"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: ruff-0.8
head: micha/remove-syntax-error-rule-code
created_at: 2024-11-18T10:28:34Z
updated_at: 2024-11-19T12:32:39Z
url: https://github.com/astral-sh/ruff/pull/14428
synced_at: 2026-01-12T15:55:47Z
```

# Remove the deprecated `E999` rule code

---

_@MichaReiser_

## Summary

The `E999` rule code was deprecated as part of Ruff 0.5. 

This PR now removes it entirely. Using `E999` is now a hard error.

## Test Plan

`cargo test`


---

_Label `breaking` added by @MichaReiser on 2024-11-18 10:28_

---

_Label `configuration` added by @MichaReiser on 2024-11-18 10:28_

---

_Comment by @github-actions[bot] on 2024-11-18 10:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood reviewed on 2024-11-18 11:25_

Overall this LGTM. The ecosystem output is noisy because you changed the target branch after filing the PR, but it doesn't look like there's anything E999-related in there!

I think there's still two references to E999 elsewhere in the repo that we should get rid of:

```
crates/ruff/tests/integration_test.rs:    // Select any rule except for `E999`, syntax error should still be shown.
crates/ruff_workspace/src/configuration.rs:                if matches!(rule.as_str(), "E999") {
```

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-18 11:37_

---

_Marked ready for review by @MichaReiser on 2024-11-19 08:17_

---

_@AlexWaygood approved on 2024-11-19 12:12_

---

_Merged by @AlexWaygood on 2024-11-19 12:13_

---

_Closed by @AlexWaygood on 2024-11-19 12:13_

---

_Branch deleted on 2024-11-19 12:13_

---
