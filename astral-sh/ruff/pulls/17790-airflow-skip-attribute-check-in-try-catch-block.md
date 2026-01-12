```yaml
number: 17790
title: "[`airflow`] Skip attribute check in try catch block (`AIR301`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: try-catch-airflow-rules
created_at: 2025-05-02T11:10:40Z
updated_at: 2025-05-05T14:01:05Z
url: https://github.com/astral-sh/ruff/pull/17790
synced_at: 2026-01-12T15:56:05Z
```

# [`airflow`] Skip attribute check in try catch block (`AIR301`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Skip attribute check in try catch block (`AIR301`)

## Test Plan

<!-- How was it tested? -->
update `crates/ruff_linter/resources/test/fixtures/airflow/AIR301_names_try.py`

---

_Comment by @github-actions[bot] on 2025-05-02 11:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-05-03 14:20_

---

_Label `rule` added by @MichaReiser on 2025-05-03 14:20_

---

_Label `preview` added by @MichaReiser on 2025-05-03 14:20_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/helpers.rs`:103 on 2025-05-05 09:33_

If I want to apply it to `ProviderReplacement` and other types, is there a smarter way to do so?

---

_@Lee-W reviewed on 2025-05-05 09:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/helpers.rs`:103 on 2025-05-05 13:20_

Do you mean pass `ProviderReplacement` to this function instead of `Replacement`? In that case, I see two options:
- Make the function generic and introduce some kind of `Replacement` trait:
  ```rust
  fn try_block_contains_undeprecated_attribute<T: Replacement>(
       try_node: &StmtTry,
       replacement: &T,
       semantic: &SemanticModel,
  ) -> bool { /* ... */ }
  
  trait Replacement {
      fn module(&self) -> &str;
      fn name(&self) -> &str;
   }
  ```
- Pass `module` and `name` directly to the function:
  ```rust
  fn try_block_contains_undeprecated_attribute(
       try_node: &StmtTry,
       module: &str,
       name: &str,
       semantic: &SemanticModel,
  ) -> bool { /* ... */ }
  ```

These both assume that you just need `module` and `name` from the `Replacement` types, but that seems to be the case. I think the second option is a bit easier if there are only a few fields. A trait is probably overkill.

---

_@ntBre approved on 2025-05-05 13:24_

Thanks, this looks good to me! Is this good to merge, or do you want to resolve your question about `ProviderReplacement` first?

---

_Comment by @Lee-W on 2025-05-05 13:59_

> Thanks, this looks good to me! Is this good to merge, or do you want to resolve your question about `ProviderReplacement` first?

Would be nice if we could merge it first and I'll spend some time figure that out tomorrow 🙂 Thanks for the suggestions!

---

_Merged by @ntBre on 2025-05-05 14:01_

---

_Closed by @ntBre on 2025-05-05 14:01_

---
