```yaml
number: 9423
title: "[flake8-pyi] PYI053: Exclude string literals that are the first argument to `warnings.deprecated` or `typing_extensions.deprecated`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
assignees: []
merged: true
base: main
head: depr-pyi053
created_at: 2024-01-07T17:27:20Z
updated_at: 2024-01-07T23:43:52Z
url: https://github.com/astral-sh/ruff/pull/9423
synced_at: 2026-01-10T22:57:09Z
```

# [flake8-pyi] PYI053: Exclude string literals that are the first argument to `warnings.deprecated` or `typing_extensions.deprecated`

---

_Pull request opened by @AlexWaygood on 2024-01-07 17:27_

Fixes #9420

---

_@charliermarsh reviewed on 2024-01-07 17:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:83 on 2024-01-07 17:31_

Nit: I'd recommend passing in `semantic: &SemanticModel` here, since it's a much lighter dependency than `&Checker` (and makes the function's dependencies much clearer).

---

_@charliermarsh reviewed on 2024-01-07 17:35_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:57 on 2024-01-07 17:35_

I think for simplicity I might suggest structuring this like `is_docstring_stmt` -- so just return `true` if we're in one of those decorators, rather than comparing the literal.

---

_@charliermarsh reviewed on 2024-01-07 17:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:82 on 2024-01-07 17:36_

You can use the `?` operator to simplify some of these returns a little. For example:

```
let call = expr.as_call_expr()?;
semantic.resolve_call_path(call.func.as_ref()).is_some_and(...)
```


---

_@AlexWaygood reviewed on 2024-01-07 17:36_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:82 on 2024-01-07 17:36_

Oh, of course

---

_@AlexWaygood reviewed on 2024-01-07 17:43_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:57 on 2024-01-07 17:43_

Yeah, I wondered about that. It would make it simpler, but could possibly lead to false negatives if people are (incorrectly) passing in string literals as a second or third argument to `@deprecated`? Only the first argument should really be excluded. But I'll admit it's unlikely that people would pass in string literals as a second or third argument of `@deprecated`, since it only has one non-keyword-only argument at runtime. So maybe the simpler solution is better here. I'll defer to your judgement :)

---

_Comment by @github-actions[bot] on 2024-01-07 18:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-01-07 19:08_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:57 on 2024-01-07 19:08_

Hmm I see. I'd be okay with the simpler method, but if we retain this, I think it needs to be tweaked a bit, since `StringLike` can be an f-string or a bytes literal. Instead of comparing based on type and contents, can we just compare the ranges to see if the first argument is `string`?

---

_@AlexWaygood reviewed on 2024-01-07 21:24_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:57 on 2024-01-07 21:24_

Yeah, I changed my mind -- have amended the PR to go with the simpler approach :)

I think I was worrying to much -- declining to emit this error on any arguments inside a call to `@warnings.deprecated` should be fine. False negatives are obviously less bad than false positives in general, but that feels especially true here: the whole premise of PYI053 is basically a somewhat fuzzy heuristic for spotting some questionably long default values in stubs.

Plus, it feels really unlikely that anybody would trigger this hypothetical false negative in practice (since you'd have to write code that type checkers would flag, and runtime Python would reject). _And_ flake8-pyi also just ignores all string literals inside calls to `@warnings.deprecated`, so we'll be consistent with the original Python implementation of this rule :)

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-01-07 21:24_

---

_@charliermarsh approved on 2024-01-07 23:32_

Great, thank you!

---

_Merged by @charliermarsh on 2024-01-07 23:41_

---

_Closed by @charliermarsh on 2024-01-07 23:41_

---

_@charliermarsh reviewed on 2024-01-07 23:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:57 on 2024-01-07 23:42_

I definitely could've gone either way here -- it seemed "simple enough" to me that it was a better complexity tradeoff to just go with this approach, but I can also see how it would be subtly wrong in some ways.

---

_Label `bug` added by @charliermarsh on 2024-01-07 23:42_

---

_Branch deleted on 2024-01-07 23:43_

---
