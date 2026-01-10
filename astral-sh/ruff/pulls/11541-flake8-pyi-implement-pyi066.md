```yaml
number: 11541
title: "[`flake8-pyi`] Implement `PYI066`"
type: pull_request
state: merged
author: tusharsadhwani
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pyi-066
created_at: 2024-05-25T20:09:08Z
updated_at: 2024-06-02T19:54:24Z
url: https://github.com/astral-sh/ruff/pull/11541
synced_at: 2026-01-10T21:56:00Z
```

# [`flake8-pyi`] Implement `PYI066`

---

_Pull request opened by @tusharsadhwani on 2024-05-25 20:09_

## Summary

- Implements `Y066` from `flake8-pyi` as `PYI066`
- Fixes `PYI006` not being raised for `elif` clauses. This would have conflicted with PYI006's implementation, so decided to do it in the same PR.

## Test Plan

`cargo test` / `cargo insta review`


---

_Review requested from @AlexWaygood by @tusharsadhwani on 2024-05-25 20:09_

---

_@tusharsadhwani reviewed on 2024-05-25 20:23_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:78 on 2024-05-25 20:23_

I don't think this formatting is the best, but the `mkdocs` check is failing if I change this in any way.

---

_Comment by @github-actions[bot] on 2024-05-25 20:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-05-27 14:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:78 on 2024-05-27 14:54_

I opened https://github.com/astral-sh/ruff/issues/11568 to track this; I've also bumped into this in the past :-)

---

_@tusharsadhwani reviewed on 2024-05-27 15:02_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:78 on 2024-05-27 15:02_

feel free to revert the last commit to format it correctly as needed.

---

_Comment by @charliermarsh on 2024-05-28 23:59_

I can merge this one too.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-28 23:59_

---

_Label `rule` added by @charliermarsh on 2024-05-28 23:59_

---

_Label `preview` added by @charliermarsh on 2024-05-28 23:59_

---

_Renamed from "[flake8-pyi] Implement PYI066, fix PYI006" to "[`flake8-pyi`] Implement `PYI066`" by @charliermarsh on 2024-05-29 00:26_

---

_Merged by @charliermarsh on 2024-05-29 00:30_

---

_Closed by @charliermarsh on 2024-05-29 00:30_

---

_Comment by @charliermarsh on 2024-05-29 00:31_

Thanks @tusharsadhwani!

---

_Branch deleted on 2024-06-02 19:54_

---
