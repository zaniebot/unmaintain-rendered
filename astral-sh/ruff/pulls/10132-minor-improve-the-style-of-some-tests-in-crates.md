```yaml
number: 10132
title: "[Minor] Improve the style of some tests in `crates/ruff/tests/format.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: test-style
created_at: 2024-02-26T10:29:52Z
updated_at: 2024-02-26T11:06:12Z
url: https://github.com/astral-sh/ruff/pull/10132
synced_at: 2026-01-12T15:55:31Z
```

# [Minor] Improve the style of some tests in `crates/ruff/tests/format.rs`

---

_@AlexWaygood_

While #9599 was being reviewed, @zanieb taught me about the useful `insta::with_settings` technique you can use to dynamically insert some content into an `insta_cmd` snapshot. I thought I had switched all my tests in that PR to use `insta::with_settings` in https://github.com/astral-sh/ruff/pull/9599/commits/7739440e40eaebab41b3fed68a0e25b9b458777e, but looking back, I only fixed the tests I added in `crates/ruff/tests/lint.rs`. As a result, we now have some tests in `crates/ruff/tests/format.rs` that use raw `assert_eq` calls, which lead to less informative tracebacks when the test fails. This PR fixes that.

---

_Comment by @github-actions[bot] on 2024-02-26 10:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2024-02-26 11:02_

---

_Closed by @AlexWaygood on 2024-02-26 11:02_

---

_Branch deleted on 2024-02-26 11:02_

---

_Comment by @MichaReiser on 2024-02-26 11:06_

Nice

---
