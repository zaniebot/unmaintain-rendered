```yaml
number: 11325
title: "[`flake8-pyi`] Implement `PYI064`"
type: pull_request
state: merged
author: tusharsadhwani
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pyi-064
created_at: 2024-05-07T18:08:24Z
updated_at: 2024-06-02T22:33:46Z
url: https://github.com/astral-sh/ruff/pull/11325
synced_at: 2026-01-12T15:55:37Z
```

# [`flake8-pyi`] Implement `PYI064`

---

_@tusharsadhwani_

## Summary

Implements `Y064` from `flake8-pyi` and its autofix.

## Test Plan

`cargo test` / `cargo insta review`


---

_Review requested from @AlexWaygood by @tusharsadhwani on 2024-05-07 18:08_

---

_@tusharsadhwani reviewed on 2024-05-07 18:09_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI064.py`:13 on 2024-05-07 18:09_

I think an unsafe autofix is appropriate here (specifically when the assignment value is different from the literal value).

Is autofixing it to `w2: Final = 123` in unsafe mode fair?

---

_@AlexWaygood reviewed on 2024-05-07 18:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI064.py`:13 on 2024-05-07 18:31_

ummmmmmm this kind of thing doesn't really make any sense, right? I think I'd lean towards not doing an autofix here -- "refuse the temptation to guess"

---

_@tusharsadhwani reviewed on 2024-05-07 18:32_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI064.py`:13 on 2024-05-07 18:32_

fair enough.

---

_@tusharsadhwani reviewed on 2024-05-07 18:51_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI064.py`:13 on 2024-05-07 18:51_

done.

---

_Comment by @github-actions[bot] on 2024-05-07 19:07_

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

_@tusharsadhwani reviewed on 2024-05-07 19:33_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_final_literal.rs`:81 on 2024-05-07 19:33_

can potentially be moved to `ruff_python_ast/src/helpers.rs`, as a `is_literal_expr` helper.

---

_@tusharsadhwani reviewed on 2024-05-07 19:50_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI064.py`:21 on 2024-05-07 19:50_

Right now this mimics `flake8-pyi` behaviour, however I think raising the issue here should be fine?

---

_Comment by @charliermarsh on 2024-05-28 23:27_

@AlexWaygood -- will follow-up and merge this.

---

_Label `rule` added by @charliermarsh on 2024-05-28 23:27_

---

_Label `preview` added by @charliermarsh on 2024-05-28 23:27_

---

_Renamed from "[flake8-pyi] Implement PYI064" to "[`flake8-pyi`] Implement `PYI064`" by @charliermarsh on 2024-05-28 23:27_

---

_@charliermarsh approved on 2024-05-28 23:52_

---

_Merged by @charliermarsh on 2024-05-28 23:57_

---

_Closed by @charliermarsh on 2024-05-28 23:57_

---

_Branch deleted on 2024-06-02 19:54_

---

_@Fffjjy reviewed on 2024-06-02 22:32_

Fix

---
