```yaml
number: 13250
title: "refactor: remove unnecessary string hashes"
type: pull_request
state: merged
author: hamirmahal
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor/remove-unnecessary-string-hashes
created_at: 2024-09-04T23:53:29Z
updated_at: 2024-09-18T18:30:06Z
url: https://github.com/astral-sh/ruff/pull/13250
synced_at: 2026-01-12T15:55:43Z
```

# refactor: remove unnecessary string hashes

---

_@hamirmahal_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This removes unnecessary `#`s in parts of the codebase to improve readability and maintainability.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

`cargo test` passes locally.

<!-- How was it tested? -->

---

_Review requested from @carljm by @hamirmahal on 2024-09-04 23:53_

---

_Review requested from @MichaReiser by @hamirmahal on 2024-09-04 23:53_

---

_Review requested from @AlexWaygood by @hamirmahal on 2024-09-04 23:53_

---

_Comment by @github-actions[bot] on 2024-09-05 00:12_

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

_Label `internal` added by @dhruvmanila on 2024-09-05 05:39_

---

_Comment by @dhruvmanila on 2024-09-05 05:40_

Can you run `cargo fmt`?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-09-05 07:12_

---

_@hamirmahal reviewed on 2024-09-05 21:15_

I ran `cargo fmt --all`.

---

_Comment by @charliermarsh on 2024-09-06 02:50_

I think we've hit a `rustfmt` bug. Every time you run `cargo fmt`, it indents the snapshots further.

---

_Comment by @hamirmahal on 2024-09-06 18:03_

Right, that seems odd. I also haven't seen `cargo fmt` fail for any recent pushes to the repository or its pull requests, except this one.

---

_Comment by @MichaReiser on 2024-09-18 07:30_

Interesting cargo-fmt bug. I don't mind the unnecessary hashes that much. They have the benefit that it's easier to update the tests when line breaks are needed. 

I currently don't see how we can land this PR in its current form considering the cargo fmt issue. I leave it up to you if you want to close the PR or revert the changes to `lint.rs` and land the changes to the remaining files.

---

_Merged by @MichaReiser on 2024-09-18 17:08_

---

_Closed by @MichaReiser on 2024-09-18 17:08_

---

_Branch deleted on 2024-09-18 18:30_

---
