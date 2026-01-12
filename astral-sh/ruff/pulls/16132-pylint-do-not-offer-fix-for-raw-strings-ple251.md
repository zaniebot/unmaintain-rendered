```yaml
number: 16132
title: "[`pylint`] Do not offer fix for raw strings (`PLE251`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: PLE251
created_at: 2025-02-13T06:49:37Z
updated_at: 2025-02-24T09:02:07Z
url: https://github.com/astral-sh/ruff/pull/16132
synced_at: 2026-01-12T15:55:53Z
```

# [`pylint`] Do not offer fix for raw strings (`PLE251`)

---

_@InSyncWithFoo_

## Summary

Resolves #13294, follow-up to #13882.

At #13882, it was concluded that a fix should not be offered for raw strings. This change implements that. The five rules in question are now no longer always fixable.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-02-13 06:49_

---

_Review requested from @dhruvmanila by @InSyncWithFoo on 2025-02-13 06:49_

---

_Comment by @github-actions[bot] on 2025-02-13 06:58_

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

_Label `rule` added by @MichaReiser on 2025-02-13 08:20_

---

_Label `fixes` added by @MichaReiser on 2025-02-13 08:20_

---

_Label `rule` removed by @MichaReiser on 2025-02-13 08:20_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/invalid_string_characters.rs`:222 on 2025-02-13 08:23_

This doesn't seem to create an `Err` unless I'm missing something, I'd remove the `try_set_optional_fix` and just inline the logic:
```rs
if !token.is_raw_string() {
	diagnostic.set_fix(
		Fix::safe_edit(Edit::range_replacement(replacement.to_string(), range))
	);
}
```

---

_@dhruvmanila reviewed on 2025-02-13 08:23_

---

_@MichaReiser approved on 2025-02-13 08:24_

---

_@dhruvmanila reviewed on 2025-02-13 08:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token.rs`:71 on 2025-02-13 08:24_

We should make sure that the invariants are being followed here similar to other methods that operates on string tokens i.e., assert that the token is a string. We should include the panic case in the documentation.

---

_Merged by @MichaReiser on 2025-02-13 08:36_

---

_Closed by @MichaReiser on 2025-02-13 08:36_

---

_Branch deleted on 2025-02-13 13:40_

---

_Label `bug` added by @dhruvmanila on 2025-02-24 09:02_

---
