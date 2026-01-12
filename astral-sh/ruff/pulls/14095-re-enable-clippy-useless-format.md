```yaml
number: 14095
title: "Re-enable clippy `useless-format`"
type: pull_request
state: merged
author: sbrugman
labels:
  - internal
assignees: []
merged: true
base: main
head: re-enable-clippy-useless-format
created_at: 2024-11-04T17:14:14Z
updated_at: 2024-11-04T17:37:05Z
url: https://github.com/astral-sh/ruff/pull/14095
synced_at: 2026-01-12T15:55:46Z
```

# Re-enable clippy `useless-format`

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

@MichaReiser was faster with merging than I could push this final commit on "useless" format calls 😄 (https://github.com/astral-sh/ruff/pull/14093)

Now that we've eliminated most `format!` calls without arguments, we can re-enable the clippy lint:

https://rust-lang.github.io/rust-clippy/master/#useless_format

## Test Plan

`cargo clippy --workspace --all-targets --all-features -- -D warnings`

---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-04 17:14_

---

_Renamed from "Re-enable cliipy `useless-format`" to "Re-enable clippy `useless-format`" by @sbrugman on 2024-11-04 17:14_

---

_Comment by @MichaReiser on 2024-11-04 17:17_

Whoops sorry.

---

_Label `internal` added by @MichaReiser on 2024-11-04 17:17_

---

_@AlexWaygood approved on 2024-11-04 17:18_

---

_Merged by @MichaReiser on 2024-11-04 17:25_

---

_Closed by @MichaReiser on 2024-11-04 17:25_

---

_Comment by @github-actions[bot] on 2024-11-04 17:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2024-11-04 17:37_

---
