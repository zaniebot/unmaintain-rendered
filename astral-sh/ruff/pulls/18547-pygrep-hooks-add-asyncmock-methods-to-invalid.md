```yaml
number: 18547
title: "[`pygrep_hooks`] Add `AsyncMock` methods to `invalid-mock-access` (`PGH005`)"
type: pull_request
state: merged
author: robsdedude
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/pgh005-add-async-mock-methods
created_at: 2025-06-08T08:49:51Z
updated_at: 2025-06-24T21:32:42Z
url: https://github.com/astral-sh/ruff/pull/18547
synced_at: 2026-01-12T15:56:21Z
```

# [`pygrep_hooks`] Add `AsyncMock` methods to `invalid-mock-access` (`PGH005`)

---

_@robsdedude_

## Summary
This PR expands PGH005 to also check for AsyncMock methods in the same vein. E.g., currently `assert mock.not_called` is linted. This PR adds the corresponding async assertions `assert mock.not_awaited()`.


---

_@robsdedude reviewed on 2025-06-08 08:52_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/pygrep_hooks/rules/invalid_mock_access.rs`:99 on 2025-06-08 08:52_

N.B., the sync equivalent `called` is not in this list. That is because (sync) `Mock`s have a boolean attribute `called`. However, `AsyncMock`s do not have an attribute `awaited`. Therefore, `assert mock.awaited` is likely a mistake and was meant to be `mock.assert_awaited()`.

---

_@robsdedude reviewed on 2025-06-08 08:55_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/pygrep_hooks/rules/invalid_mock_access.rs`:1 on 2025-06-08 08:55_

I just noticed that this rule is already stabilized. So adding this change (specifically given that is has the possibility of producing false-positives) is no good. Maybe it needs to go into a separate rule or such? What is the right process here?

---

_Comment by @github-actions[bot] on 2025-06-08 08:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-06-09 21:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pygrep_hooks/rules/invalid_mock_access.rs`:1 on 2025-06-09 21:28_

I haven't given this a review, just passing by, so this might not be the right suggestion, but we can also add new rule _behaviors_ in preview. In that case you just gate the new part of the rule behind a preview check instead of the whole rule. You can see the functions in [`preview.rs`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/preview.rs#L1) (and their references) for examples. We started gathering them like this to make them easier to find and stabilize later.

---

_Label `rule` added by @ntBre on 2025-06-09 21:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/preview.rs`:94 on 2025-06-18 18:13_

```suggestion
// https://github.com/astral-sh/ruff/pull/18547
pub(crate) const fn is_invalid_async_mock_access_check_enabled(settings: &LinterSettings) -> bool {
```

This is helpful when reviewing stabilizations in the future.

---

_@ntBre approved on 2025-06-18 18:23_

Makes sense, thanks! Just one tiny suggestion to include the URL for the PR on the preview function.

---

_Label `preview` added by @ntBre on 2025-06-18 18:23_

---

_@ntBre approved on 2025-06-24 21:26_

---

_Renamed from "[`pygrep_hooks`] add AsyncMock methods to `invalid-mock-access` (`PGH005`)" to "[`pygrep_hooks`] Add `AsyncMock` methods to `invalid-mock-access` (`PGH005`)" by @ntBre on 2025-06-24 21:27_

---

_Merged by @ntBre on 2025-06-24 21:27_

---

_Closed by @ntBre on 2025-06-24 21:27_

---

_Branch deleted on 2025-06-24 21:32_

---
