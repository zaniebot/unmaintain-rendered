```yaml
number: 15438
title: "[`flake8-pie`] Reuse parsed tokens (`PIE800`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - internal
assignees: []
merged: true
base: main
head: PIE800
created_at: 2025-01-12T00:14:31Z
updated_at: 2025-01-13T09:14:05Z
url: https://github.com/astral-sh/ruff/pull/15438
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-pie`] Reuse parsed tokens (`PIE800`)

---

_@InSyncWithFoo_

## Summary

Follow-up to #15394. See [this review comment](https://github.com/astral-sh/ruff/pull/15394#discussion_r1910526741).

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-01-12 00:15_

CC: @dhruvmanila.

---

_Comment by @github-actions[bot] on 2025-01-12 00:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dhruvmanila by @MichaReiser on 2025-01-12 10:21_

---

_Label `internal` added by @MichaReiser on 2025-01-12 10:21_

---

_@charliermarsh approved on 2025-01-13 01:59_

---

_Merged by @charliermarsh on 2025-01-13 02:03_

---

_Closed by @charliermarsh on 2025-01-13 02:03_

---

_Branch deleted on 2025-01-13 02:04_

---

_@dhruvmanila reviewed on 2025-01-13 06:03_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:86 on 2025-01-13 06:03_

We'll need to be careful here because the `skip_trivia` includes the `Newline` token while the `is_trivia` method on `TokenKind` does not. The latter only includes the `NonLogicalNewline` which is different than `Newline`. It _might_ be that this is ok because the `SimpleTokenizer` emits `Newline` regardless of the context.

---

_@dhruvmanila reviewed on 2025-01-13 06:07_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:86 on 2025-01-13 06:07_

I think it should be ok to do here because the edits are only generated when inside parentheses which will emit `NonLogicalNewline` instead of `Newline`.

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:86 on 2025-01-13 08:41_

I take it that this change is fine, then?

---

_@InSyncWithFoo reviewed on 2025-01-13 08:41_

---

_@dhruvmanila reviewed on 2025-01-13 09:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:86 on 2025-01-13 09:14_

Yes

---
