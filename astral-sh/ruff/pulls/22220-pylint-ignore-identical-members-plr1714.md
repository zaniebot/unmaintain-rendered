```yaml
number: 22220
title: "[`pylint`] Ignore identical members (`PLR1714`)"
type: pull_request
state: merged
author: njhearp
labels:
  - rule
assignees: []
merged: true
base: main
head: PLR1714-identical-members
created_at: 2025-12-27T01:00:20Z
updated_at: 2026-01-02T18:05:03Z
url: https://github.com/astral-sh/ruff/pull/22220
synced_at: 2026-01-10T16:36:18Z
```

# [`pylint`] Ignore identical members (`PLR1714`)

---

_Pull request opened by @njhearp on 2025-12-27 01:00_

## Summary

This PR closes #21692. `PLR1714` will no longer flag if all members are identical. I iterate through the equality comparisons and if they are all equal the rule does not flag.

## Test Plan

Additional tests were added with identical members.


---

_Comment by @astral-sh-bot[bot] on 2025-12-27 01:12_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review requested from @ntBre by @ntBre on 2025-12-29 14:18_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs`:143 on 2026-01-02 16:12_

I don't _think_ we can get here if `comparators` is empty, but it always makes me nervous to index like this, just in case, since it will panic when empty. What do you think about wrapping this check in:

```rust
if let Some(comparator) = comparators.first() {
   // ...
}
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLR1714_repeated_equality_comparison.py.snap`:476 on 2026-01-02 16:18_

Good idea to include this case! Is this the desired behavior here? To me this has the same issue as the all-members case, but maybe we should just leave it to [duplicate-value (B033)](https://docs.astral.sh/ruff/rules/duplicate-value/#duplicate-value-b033) to fix this.

I think I've talked myself into this making sense but curious if that aligns with your thinking.

---

_@ntBre reviewed on 2026-01-02 16:19_

Thank you!

I just had one nit about the indexing and a question about the second test case, but this looks great.

---

_Label `rule` added by @ntBre on 2026-01-02 16:20_

---

_Review comment by @njhearp on `crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs`:143 on 2026-01-02 16:33_

Yes, I agree that it would be best to wrap the check for safety.

---

_@njhearp reviewed on 2026-01-02 16:33_

---

_@njhearp reviewed on 2026-01-02 17:47_

---

_Review comment by @njhearp on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLR1714_repeated_equality_comparison.py.snap`:476 on 2026-01-02 17:47_

Yes, that is the desired behavior. I think leaving it to [duplicate-value (B033)](https://docs.astral.sh/ruff/rules/duplicate-value/#duplicate-value-b033) would make sense. In case you commented on, PLR1714 would flag and then B033 would take care of the duplicate.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs`:143 on 2026-01-02 17:55_

Nice!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLR1714_repeated_equality_comparison.py.snap`:476 on 2026-01-02 17:55_

Sounds good, thanks for confirming.

---

_@ntBre approved on 2026-01-02 17:55_

Thank you!

---

_Merged by @ntBre on 2026-01-02 17:56_

---

_Closed by @ntBre on 2026-01-02 17:56_

---

_@njhearp reviewed on 2026-01-02 18:03_

---

_Review comment by @njhearp on `crates/ruff_linter/resources/test/fixtures/pylint/repeated_equality_comparison.py`:77 on 2026-01-02 18:03_

I think we should consider a rule to get this to reduce to ```foo == "bar"```. Opening a separate issue would probably be best.

---

_@ntBre reviewed on 2026-01-02 18:05_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/repeated_equality_comparison.py`:77 on 2026-01-02 18:05_

I think this is possibly tracked in https://github.com/astral-sh/ruff/issues/21690. Micha mentioned a more general unreachable rule there and on https://github.com/astral-sh/ruff/issues/21692.

---
