```yaml
number: 16447
title: "[syntax-errors] Type parameter defaults before Python 3.13"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-type-param-default
created_at: 2025-02-28T23:02:31Z
updated_at: 2025-03-04T16:59:04Z
url: https://github.com/astral-sh/ruff/pull/16447
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Type parameter defaults before Python 3.13

---

_Pull request opened by @ntBre on 2025-02-28 23:02_

Summary
--

Detects the presence of a [PEP 696] type parameter default before Python 3.13.

Test Plan
--

New inline parser tests for type aliases, generic functions and generic classes.

[PEP 696]: https://peps.python.org/pep-0696/#grammar-changes


---

_Label `preview` added by @ntBre on 2025-02-28 23:02_

---

_Review requested from @MichaReiser by @ntBre on 2025-02-28 23:02_

---

_Review requested from @dhruvmanila by @ntBre on 2025-02-28 23:02_

---

_Comment by @github-actions[bot] on 2025-02-28 23:11_

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

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@type_param_default_py312.py.snap`:182 on 2025-03-03 05:38_

What do you think about highlighting only the default type part as that's what is causing the syntax error? So,
```
  |
1 | # parse_options: {"target-version": "3.12"}
2 | type X[T = int] = int
  |            ^^^ Syntax Error: Cannot use type parameter default on Python 3.12 (syntax was added in Python 3.13)
3 | def f[T = int](): ...
4 | class C[T = int](): ...
  |
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@type_param_default_py312.py.snap`:189 on 2025-03-03 05:45_

What about something like "Cannot set default type for a type parameter on Python 3.12 (...)" ?

---

_@dhruvmanila approved on 2025-03-03 05:45_

---

_Label `parser` added by @dhruvmanila on 2025-03-03 05:45_

---

_@MichaReiser reviewed on 2025-03-03 07:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@type_param_default_py312.py.snap`:182 on 2025-03-03 07:50_

I like this. Should we extend the range to include the `=` as well?

---

_@MichaReiser approved on 2025-03-03 07:50_

---

_Comment by @ntBre on 2025-03-03 15:55_

Thanks! I restricted the diagnostic range to the `= ...` part, updated the wording of the message, and also threw in a test case with a non-default parameter and a second default parameter just to make sure the ranges still worked. I think the more precise range looks a lot better.

---

_@dhruvmanila reviewed on 2025-03-04 11:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3245 on 2025-03-04 11:41_

nit: I think it's now the start of the `=` token instead so like `equal_token_start`?

---

_@dhruvmanila approved on 2025-03-04 11:41_

---

_Merged by @ntBre on 2025-03-04 16:53_

---

_Closed by @ntBre on 2025-03-04 16:53_

---

_Branch deleted on 2025-03-04 16:53_

---
