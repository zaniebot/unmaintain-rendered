```yaml
number: 17050
title: "Don't install Rust before running pre-commit in CI"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/precommit-faster
created_at: 2025-03-28T20:51:17Z
updated_at: 2025-03-28T21:00:16Z
url: https://github.com/astral-sh/ruff/pull/17050
synced_at: 2026-01-12T15:56:00Z
```

# Don't install Rust before running pre-commit in CI

---

_@AlexWaygood_

Following https://github.com/astral-sh/ruff/commit/29573daef5a7587b7903095a8c6be03739cb1ccb, it doesn't look to me like any of the pre-commit hooks run in CI here are Rust-based:

- `cargo fmt` is Rust-based but it's explicitly skipped as part of this job and run as a separate CI job: https://github.com/astral-sh/ruff/blob/93052331b07e8cbc84660340019db4f582fb4ad1/.pre-commit-config.yaml#L124-L125
- The `typos` hook is Rust-based, but according to https://github.com/crate-ci/typos/blob/master/docs/pre-commit.md pre-commit should install a built binary rather than building the binary from source

As such, I think this step in the workflow is just taking up 15s of CI time and not actually speeding up pre-commit at all

---

_Label `ci` added by @AlexWaygood on 2025-03-28 20:51_

---

_@MichaReiser approved on 2025-03-28 20:57_

---

_Comment by @AlexWaygood on 2025-03-28 20:57_

I confirmed that this doesn't slow down at all the step where we're actually running pre-commit

---

_Merged by @AlexWaygood on 2025-03-28 20:57_

---

_Closed by @AlexWaygood on 2025-03-28 20:57_

---

_Branch deleted on 2025-03-28 20:57_

---

_Comment by @github-actions[bot] on 2025-03-28 21:00_

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
