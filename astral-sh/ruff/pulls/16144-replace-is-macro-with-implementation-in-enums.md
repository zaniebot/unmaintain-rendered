```yaml
number: 16144
title: Replace is-macro with implementation in enums
type: pull_request
state: merged
author: Glyphack
labels:
  - internal
assignees: []
merged: true
base: main
head: replace-is-macro-with-codegen
created_at: 2025-02-13T20:10:51Z
updated_at: 2025-02-14T05:07:21Z
url: https://github.com/astral-sh/ruff/pull/16144
synced_at: 2026-01-12T15:55:53Z
```

# Replace is-macro with implementation in enums

---

_@Glyphack_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Replace usage of is_macro in generated AST models with implementation of those methods.
Addresses this comment: https://github.com/astral-sh/ruff/pull/15544/files#r1919735841

The implementation is straightforward, there are just two decisions:

1. For enums with one variant we emit the catch all cases in the match. Otherwise there is a clippy warning.
2. All `impl` blocks have allow `dead_code` and [`match_wildcard_for_single_variants`](https://rust-lang.github.io/rust-clippy/master/index.html#match_wildcard_for_single_variants). Some of the methods are unused. Also I don't think we need to care about catch all case since the methods are obviously matching for one thing. I can also make this per function but I decided do it on the `impl` to generate smaller file.

Sadly we cannot remove the dependency yet, but we can if we replace more of the ast files with auto generated code.
## Test Plan

No other file has changed. And the compilation and tests pass.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-02-13 20:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/c6a192bb461d0bc317b61659254ef76582b4ce9c/rotkehlchen/utils/mixins/enums.py#L12'>rotkehlchen/utils/mixins/enums.py:12:16:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI019 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @Glyphack on 2025-02-13 20:49_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:715 on 2025-02-13 21:55_

This change seems unrelated ;)

---

_@MichaReiser approved on 2025-02-13 21:55_

Thanks. This is great. Fewer usages of the `is_macro` makes me very happy

---

_Label `internal` added by @MichaReiser on 2025-02-13 21:55_

---

_@Glyphack reviewed on 2025-02-13 22:06_

---

_Review comment by @Glyphack on `.github/workflows/ci.yaml`:715 on 2025-02-13 22:06_

It's resolved now after merging main.

---

_Merged by @MichaReiser on 2025-02-13 22:49_

---

_Closed by @MichaReiser on 2025-02-13 22:49_

---

_Branch deleted on 2025-02-14 05:07_

---
