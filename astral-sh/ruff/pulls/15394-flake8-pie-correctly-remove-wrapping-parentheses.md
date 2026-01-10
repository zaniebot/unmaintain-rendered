```yaml
number: 15394
title: "[`flake8-pie`] Correctly remove wrapping parentheses (`PIE800`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: PIE800
created_at: 2025-01-10T09:52:20Z
updated_at: 2025-01-10T15:24:49Z
url: https://github.com/astral-sh/ruff/pull/15394
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-pie`] Correctly remove wrapping parentheses (`PIE800`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-10 09:52_

## Summary

Resolves #15366.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-10 09:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2025-01-10 14:07_

---

_Label `fixes` added by @MichaReiser on 2025-01-10 14:07_

---

_Merged by @MichaReiser on 2025-01-10 14:52_

---

_Closed by @MichaReiser on 2025-01-10 14:52_

---

_Branch deleted on 2025-01-10 14:53_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:82 on 2025-01-10 15:24_

I haven't looked at the complete solution but I'm wondering if `SimpleTokenizer` is required when we have access to the parsed tokens. You can access tokens for a [specific range](https://github.com/astral-sh/ruff/blob/6e9ff445fd8559972b423370de20563a9c2db8d4/crates/ruff_python_parser/src/lib.rs#L419-L419) or [all tokens after a specific offset](https://github.com/astral-sh/ruff/blob/6e9ff445fd8559972b423370de20563a9c2db8d4/crates/ruff_python_parser/src/lib.rs#L457-L457). Refer to the references of those methods as an example on how to use them.

---

_@dhruvmanila reviewed on 2025-01-10 15:24_

---

_@dhruvmanila reviewed on 2025-01-10 15:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:82 on 2025-01-10 15:24_

Oh, this rule already utilized `SimpleTokenizer` from before. Hmm, I wonder if we can remove that and switch to just getting the lexed tokens.

---
