```yaml
number: 22347
title: "Error on unused `ty: ignore` comments when dogfooding ty on our own scripts"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/unused-ignore-comment-dogfooding
created_at: 2026-01-02T20:23:23Z
updated_at: 2026-01-03T10:32:18Z
url: https://github.com/astral-sh/ruff/pull/22347
synced_at: 2026-01-12T15:57:47Z
```

# Error on unused `ty: ignore` comments when dogfooding ty on our own scripts

---

_@AlexWaygood_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @AlexWaygood on 2026-01-02 20:23_

---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-02 20:23_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-02 20:23_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-02 20:23_

---

_Label `internal` added by @AlexWaygood on 2026-01-02 20:23_

---

_Merged by @AlexWaygood on 2026-01-02 20:27_

---

_Closed by @AlexWaygood on 2026-01-02 20:27_

---

_Branch deleted on 2026-01-02 20:27_

---

_@MichaReiser reviewed on 2026-01-03 10:32_

---

_Review comment by @MichaReiser on `scripts/pyproject.toml`:20 on 2026-01-03 10:32_

I personally prefer `warn` with `error-on-warning` because it makes those errors look less scary in the editor.

---
