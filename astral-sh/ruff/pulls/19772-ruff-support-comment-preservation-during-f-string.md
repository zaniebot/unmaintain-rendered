```yaml
number: 19772
title: "[`ruff`] Support comment preservation during f-string conversion (`RU…"
type: pull_request
state: open
author: mikeleppane
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix(#19745)/RUF010-fix-deletes-comment
created_at: 2025-08-05T20:59:42Z
updated_at: 2025-08-06T17:15:03Z
url: https://github.com/astral-sh/ruff/pull/19772
synced_at: 2026-01-12T15:56:46Z
```

# [`ruff`] Support comment preservation during f-string conversion (`RU…

---

_@mikeleppane_


## Summary

Previously, the rule (RUF010) would silently remove comments when transforming expressions like `f"{ascii(# comment\n1)}"` to `f"{1!a}"`. This change adds a slightly improved comment analysis to distinguish between safe and unsafe transformations. `explicit_f_string_type_conversion`

## Changes
- **Added comment preservation analysis**: New function analyzes whether comments within f-string expressions can be preserved during transformation `analyze_comment_preservation`
- **Safe vs unsafe fix classification**:
    - **Safe fixes**: Comments within function argument parentheses are preserved (e.g., `f"{ascii((# comment\n1))}"` → `f"{(# comment\n1)!a}"`)
    - **Unsafe fixes**: Comments between function name and arguments would be lost (e.g., `f"{ascii # comment\n(1)}"` → `f"{1!a}"`)

## Examples
**Safe transformations** (comments preserved):
``` python
f"{ascii((  # comment inside
    1
))}"  # → f"{(  # comment inside\n    1\n)!a}"
```
**Unsafe transformations** (comments lost, requires ): `--unsafe-fixes`
``` python
f"{ascii  # comment here
    (1)}"  # → f"{1!a}"  # comment disappears
```
This ensures users have explicit control over potentially destructive transformations while maximizing the cases where safe automatic fixes can be applied.


See: [ISSUE](https://github.com/astral-sh/ruff/issues/19745)

## Test Plan

```bash
cargo test
```


---

_Comment by @github-actions[bot] on 2025-08-05 21:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-08-06 17:08_

---

_Label `fixes` added by @ntBre on 2025-08-06 17:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:184 on 2025-08-06 17:12_

I think we should be able to use our existing `Applicability` type here:

https://github.com/astral-sh/ruff/blob/ec5660d78613352cb618d296f923d6ed086ee542/crates/ruff_diagnostics/src/fix.rs#L14

it integrates with `Fix` via `Fix::applicable_edit`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:247 on 2025-08-06 17:13_

Similarly, we have code for checking comment ranges. With the range of the replacement, which we have above, you should just be able to use `CommentRanges::interesects`. Here's a complete example of what this usually looks like in another rule:

https://github.com/astral-sh/ruff/blob/4d57fcd5a488e628aa26aaacd0a61728609ff310/crates/ruff_linter/src/rules/perflint/rules/unnecessary_list_cast.rs#L148-L155

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF010.py`:38 on 2025-08-06 17:14_

nit: can you revert this? It makes the diff a lot easier to review if the existing snapshots don't shift around.

---

_@ntBre requested changes on 2025-08-06 17:15_

---
