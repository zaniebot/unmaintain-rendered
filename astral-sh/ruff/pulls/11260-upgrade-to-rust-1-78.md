```yaml
number: 11260
title: Upgrade to Rust 1.78
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: rust-1.78
created_at: 2024-05-03T07:35:55Z
updated_at: 2024-05-03T12:56:03Z
url: https://github.com/astral-sh/ruff/pull/11260
synced_at: 2026-01-12T15:55:37Z
```

# Upgrade to Rust 1.78

---

_@MichaReiser_

## Summary

Sets the local toolchain to 1.78. This PR does not update the minimal required rust version.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-05-03 07:36_

---

_@MichaReiser reviewed on 2024-05-03 07:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:98 on 2024-05-03 07:40_

The function was never called, :hand_over_mouth: 

I decided to instead inline the `debug_assertion` in `fmt` and test that all dangling comments were formatted.

---

_Comment by @github-actions[bot] on 2024-05-03 08:08_

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

_Review comment by @dhruvmanila on `crates/ruff/src/args.rs`:341 on 2024-05-03 08:46_

nit: do we need the quotes as well?

---

_Review comment by @dhruvmanila on `crates/ruff/src/args.rs`:469 on 2024-05-03 08:46_

nit: (same as above) do we need the quotes as well?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/directives.rs`:276 on 2024-05-03 09:05_

nit: this might look a bit weird. Is `Indexer::comment_ranges` possible?

---

_Review comment by @dhruvmanila on `crates/ruff_formatter/src/printer/queue.rs`:198 on 2024-05-03 09:06_

nit: what happened here? :)

---

_Review comment by @dhruvmanila on `docs/configuration.md`:614 on 2024-05-03 09:08_

nit: same as above, do we need both quote and the back tick?

---

_@dhruvmanila approved on 2024-05-03 09:08_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/printer/queue.rs`:198 on 2024-05-03 09:27_

It's unused

---

_@MichaReiser reviewed on 2024-05-03 09:27_

---

_@MichaReiser reviewed on 2024-05-03 09:27_

---

_Review comment by @MichaReiser on `docs/configuration.md`:614 on 2024-05-03 09:27_

I think the quotes are intentional to make it clear that these are string values. The backticks ensure that they're rendered as code.

---

_@AlexWaygood approved on 2024-05-03 11:18_

---

_@MichaReiser reviewed on 2024-05-03 12:36_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:341 on 2024-05-03 12:36_

I don't think they're needed here

---

_Merged by @MichaReiser on 2024-05-03 12:46_

---

_Closed by @MichaReiser on 2024-05-03 12:46_

---

_Branch deleted on 2024-05-03 12:46_

---
