```yaml
number: 18733
title: "Document link between `import-outside-top-level (PLC0415)` and `lint.flake8-tidy-imports.banned-module-level-imports`"
type: pull_request
state: merged
author: Avasam
labels:
  - documentation
assignees: []
merged: true
base: main
head: document-link-between-import-outside-top-level-PLC0415-and-lint_flake8-tidy-imports_banned-module-level-imports
created_at: 2025-06-17T19:15:23Z
updated_at: 2025-07-03T14:41:38Z
url: https://github.com/astral-sh/ruff/pull/18733
synced_at: 2026-01-12T15:56:24Z
```

# Document link between `import-outside-top-level (PLC0415)` and `lint.flake8-tidy-imports.banned-module-level-imports`

---

_@Avasam_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

As mentioned in https://github.com/astral-sh/ruff/issues/18728#issuecomment-2981330666 CC @ntBre

![image](https://github.com/user-attachments/assets/ac0e9ea6-6510-48be-b775-47b30bdf7efe)
![image](https://github.com/user-attachments/assets/2a69df6f-1973-4d81-8985-9e0ce70f8175)


## Test Plan

Run the docs locally as per https://github.com/astral-sh/ruff/blob/a2cd6df429a3e76880a48a5eb8816a86c36927ed/CONTRIBUTING.md#mkdocs


---

_Review comment by @Avasam on `crates/ruff_linter/src/rules/pylint/rules/import_outside_top_level.rs`:53 on 2025-06-17 19:18_

Wasn't sure whether to put this directly above `## Example`, in `## See also` or merge it into `## Options`.

```suggestion
/// ## Options
/// - This rule will ignore import statements configured in
/// [`lint.flake8-tidy-imports.banned-module-level-imports`][banned-module-level-imports]
/// for the rule [`banned-module-level-imports`][TID253].
```

---

_@Avasam reviewed on 2025-06-17 19:18_

---

_@Avasam reviewed on 2025-06-17 19:18_

---

_Review comment by @Avasam on `crates/ruff_workspace/src/options.rs`:2000 on 2025-06-17 19:18_

I didn't make those links because they appear in the json schema.

---

_Renamed from "Document link between import-outside-top-level-PLC0415 and lint_flake8-tidy-imports_banned-module-level-imports" to "Document link between `import-outside-top-level (PLC0415)` and `lint.flake8-tidy-imports.banned-module-level-imports`" by @Avasam on 2025-06-17 19:19_

---

_@Avasam reviewed on 2025-06-17 19:21_

---

_Review comment by @Avasam on `crates/ruff_linter/src/rules/pylint/rules/import_outside_top_level.rs`:53 on 2025-06-17 19:21_

Also, I didn't validate, but is this true all the time or only if  `banned-module-level-imports (TID253)` is enabled ?

---

_Comment by @github-actions[bot] on 2025-06-17 19:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-06-17 22:44_

---

_Label `documentation` added by @ntBre on 2025-06-17 22:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/import_outside_top_level.rs`:53 on 2025-06-18 15:37_

I like having a separate section, but I'd probably put `## See also` before `## Options`.

Oh, I didn't notice this when looking at the rule initially, but this _is_ only true if TID253 is enabled. Good catch! Maybe that means we can just put the whole explanation in `See also` and not have separate `Options` since they only apply conditionally. I like your current `See also` section in that case, possibly with a slight alteration to the last line:

```rust
/// if the rule [`banned-module-level-imports`][TID253] is enabled.
```

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:2000 on 2025-06-18 15:40_

We may need the same caveat about "if TID253 is enabled" here too.

---

_@ntBre approved on 2025-06-18 15:41_

Thank you! This looks great. We might just need to clarify that TID253 has to be enabled. Good idea to check that!

---

_Comment by @Avasam on 2025-06-21 01:48_

Thanks for the review! I'm away for the weekend, I'll complete this next week. (It's not urgent anyway)

---

_@Avasam reviewed on 2025-07-03 00:54_

---

_Review comment by @Avasam on `crates/ruff_workspace/src/options.rs`:2000 on 2025-07-03 00:54_

Reworded

---

_@ntBre approved on 2025-07-03 14:06_

Looks great, thank you!

---

_Merged by @ntBre on 2025-07-03 14:11_

---

_Closed by @ntBre on 2025-07-03 14:11_

---

_Branch deleted on 2025-07-03 14:41_

---
