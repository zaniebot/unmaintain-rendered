```yaml
number: 18129
title: Switch to Rust 2024 edition
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-2024
created_at: 2025-05-16T08:57:31Z
updated_at: 2025-05-16T12:18:33Z
url: https://github.com/astral-sh/ruff/pull/18129
synced_at: 2026-01-12T15:56:13Z
```

# Switch to Rust 2024 edition

---

_@MichaReiser_

## Summary

Use the Rust 2024 edition. 

This required fixing a few lifetime errors.

The first commit fixes syntax errors (mainly that `gen` is now a keyword) and formats the files with the new edition. The second commit fixes new compile errors.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-05-16 08:57_

---

_@MichaReiser reviewed on 2025-05-16 09:04_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/macros.rs`:331 on 2025-05-16 09:04_

The extra `{` took me way too long to notice

---

_@MichaReiser reviewed on 2025-05-16 09:16_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:507 on 2025-05-16 09:16_

I tried to make the `add_rule` output the right formatting but couldn't find the relevant lines and I then decided that I don't care enough. Just run `cargo fmt` and it will look correct.

---

_Comment by @github-actions[bot] on 2025-05-16 09:29_

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

_Marked ready for review by @MichaReiser on 2025-05-16 09:31_

---

_Review requested from @carljm by @MichaReiser on 2025-05-16 09:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-16 09:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-16 09:31_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-16 09:31_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-05-16 09:31_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-05-16 09:31_

---

_@carljm approved on 2025-05-16 10:35_

Didn't review all changes, but what I spot-checked looked reasonable, and tests pass! Thank you.

---

_Merged by @MichaReiser on 2025-05-16 11:25_

---

_Closed by @MichaReiser on 2025-05-16 11:25_

---

_Branch deleted on 2025-05-16 11:25_

---

_@BurntSushi reviewed on 2025-05-16 12:18_

LGTM!

---
