```yaml
number: 9313
title: "[flake8-pyi] Implement PYI058"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pyi058
created_at: 2023-12-29T18:54:49Z
updated_at: 2024-01-01T11:39:52Z
url: https://github.com/astral-sh/ruff/pull/9313
synced_at: 2026-01-12T15:55:28Z
```

# [flake8-pyi] Implement PYI058

---

_@AlexWaygood_

## Summary

This PR implements Y058 from flake8-pyi -- this is a new flake8-pyi rule that was released as part of `flake8-pyi 23.11.0`. I've followed the Python implementation as closely as possible (see https://github.com/PyCQA/flake8-pyi/commit/858c0918a80294cf813e7a4bc1f9d45fb74df705), except that for the Ruff version, the rule also applies to `.py` files as well as for `.pyi` files. (For `.py` files, we only emit the diagnostic in very specific situations, however, as there's a much higher likelihood of emitting false positives when applying this rule to a `.py` file.)

## Test Plan

`cargo test`/`cargo insta review`


---

_Comment by @github-actions[bot] on 2023-12-29 19:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2023-12-31 19:52_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:76 on 2023-12-31 19:52_

Nit: it's typically better to take `&[T]` over `&Vec<T>` -- in this case, `&[ast::Stmt]`. The compiler will let you pass in `&body` from the outside any will automatically convert to `&[ast::Stmt]`. If you have a `&Vec<T>`, you can always create a `&[T]`, but the inverse is not true -- you can't easily go from `&Vec<T>` to `&[T]`. So the `&[T]` type is more flexible, since it accepts all `&Vec<T>` but also other kinds of slice types.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:165 on 2023-12-31 19:54_

This is really common in our existing rules, but going forward, the preferred pattern here is to pass in `function_def` and access the fields within `bad_generator_return_type`, rather than passing in each field individually. (In other words: would recommend removing all these arguments, and just calling in `function_def`; you probably don't even need `stmt`).

The benefit is: the signature is much clearer, and we prevent callers from passing in bad data by accident (e.g., `body` here would accept _any_ list of statements, not _just_ a function body).

---

_@charliermarsh reviewed on 2023-12-31 19:54_

---

_@charliermarsh reviewed on 2023-12-31 19:54_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:90 on 2023-12-31 19:54_

Nit: can use `semantic` here since you already assigned it as `let semantic = checker.semantic();` above.

---

_@charliermarsh reviewed on 2023-12-31 19:55_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:90 on 2023-12-31 19:55_

As a handy tricky, you can also do `semantic.current_scope().kind.is_class()`. (This isn't a built-in Rust thing, we have a macro that adds these `is_*` methods to the enum.)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:129 on 2023-12-31 19:55_

Nit: very minor (sorry!) but looks like you have extra backticks after "Generator" and "AsyncGenerator".

---

_@charliermarsh reviewed on 2023-12-31 19:55_

---

_@charliermarsh reviewed on 2023-12-31 19:56_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:141 on 2023-12-31 19:56_

I'd probably suggest `elts.iter().skip(1).all(...)` rather than `elts[1..]`, as it avoids the potential panic of an out-of-bounds access and keeps everything within iterators.

---

_@charliermarsh reviewed on 2023-12-31 19:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:189 on 2023-12-31 19:57_

We typically put the expression as the first argument since it's the "thing the method is about", and the `semantic` as the second since it's sort of auxiliary data.

---

_@charliermarsh reviewed on 2023-12-31 19:57_

This looks great!

---

_Label `rule` added by @charliermarsh on 2023-12-31 19:57_

---

_Label `preview` added by @charliermarsh on 2023-12-31 19:57_

---

_@AlexWaygood reviewed on 2023-12-31 23:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:165 on 2023-12-31 23:05_

Ahh thanks! I was using https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs as my model for this PR, as it seemed to be a fairly similar rule

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:129 on 2023-12-31 23:07_

you can defeat the borrow checker, but you can never defeat typos apparently

---

_@AlexWaygood reviewed on 2023-12-31 23:07_

---

_Comment by @AlexWaygood on 2024-01-01 00:03_

Thanks for the review! I _think_ I tackled all your points :D

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-01-01 00:03_

---

_@charliermarsh approved on 2024-01-01 11:26_

Awesome, thanks!

---

_@charliermarsh reviewed on 2024-01-01 11:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:165 on 2024-01-01 11:28_

:thumbsup: Makes total sense. All of those rule signatures should be updated in theory but in practice it hasn’t merited doing, so more just migrating them as we go.

---

_Merged by @charliermarsh on 2024-01-01 11:28_

---

_Closed by @charliermarsh on 2024-01-01 11:28_

---

_Branch deleted on 2024-01-01 11:39_

---
