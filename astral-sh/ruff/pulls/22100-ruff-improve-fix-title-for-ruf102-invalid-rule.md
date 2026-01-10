```yaml
number: 22100
title: "[`ruff`] Improve fix title for `RUF102` invalid rule code"
type: pull_request
state: merged
author: amyreese
labels:
  - fixes
  - suppression
assignees: []
merged: true
base: main
head: amy/invalid-rule-code-message
created_at: 2025-12-19T23:09:26Z
updated_at: 2026-01-08T01:23:21Z
url: https://github.com/astral-sh/ruff/pull/22100
synced_at: 2026-01-10T16:30:32Z
```

# [`ruff`] Improve fix title for `RUF102` invalid rule code

---

_Pull request opened by @amyreese on 2025-12-19 23:09_

## Summary

Updates the fix title for RUF102 to either specify which rule code to remove, or clarify
that the entire suppression comment should be removed.

## Test Plan

Updated test snapshots.



---

_Label `fixes` added by @amyreese on 2025-12-19 23:10_

---

_Label `suppression` added by @amyreese on 2025-12-19 23:10_

---

_Review requested from @ntBre by @amyreese on 2025-12-19 23:11_

---

_Review request for @ntBre removed by @amyreese on 2025-12-19 23:11_

---

_Review requested from @MichaReiser by @amyreese on 2025-12-19 23:11_

---

_Review requested from @ntBre by @amyreese on 2025-12-19 23:11_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 23:18_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:72 on 2025-12-22 10:58_

```suggestion
            format!("Remove the rule code `{}`", self.rule_code)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions_full.snap`:388 on 2025-12-22 10:59_

Does `RUF100` need a similar treatment?

---

_@MichaReiser approved on 2025-12-22 10:59_

---

_@amyreese reviewed on 2026-01-05 19:13_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions_full.snap`:388 on 2026-01-05 19:13_

That was already addressed for RUF100 in a previous PR.

---

_Merged by @amyreese on 2026-01-08 01:23_

---

_Closed by @amyreese on 2026-01-08 01:23_

---

_Branch deleted on 2026-01-08 01:23_

---
