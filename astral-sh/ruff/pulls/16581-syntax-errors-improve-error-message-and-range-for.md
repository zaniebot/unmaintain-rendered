```yaml
number: 16581
title: "[syntax-errors] Improve error message and range for pre-PEP-614 decorator syntax errors"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: alex/syntax-error-msgs
created_at: 2025-03-09T22:32:49Z
updated_at: 2025-03-17T11:26:18Z
url: https://github.com/astral-sh/ruff/pull/16581
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Improve error message and range for pre-PEP-614 decorator syntax errors

---

_Pull request opened by @AlexWaygood on 2025-03-09 22:32_

## Summary

A small followup to https://github.com/astral-sh/ruff/pull/16386. We now tell the user exactly what it was about their decorator that constituted invalid syntax on Python <3.9, and the range now highlights the specific sub-expression that is invalid rather than highlighting the whole decorator

## Test Plan

Inline snapshots are updated, and new ones are added.


---

_Label `parser` added by @AlexWaygood on 2025-03-09 22:32_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-03-09 22:32_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-03-09 22:32_

---

_Review requested from @ntBre by @AlexWaygood on 2025-03-09 22:32_

---

_Label `preview` added by @AlexWaygood on 2025-03-09 22:33_

---

_Comment by @github-actions[bot] on 2025-03-09 22:43_

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

_@ntBre approved on 2025-03-09 23:53_

This is great, thank you!

---

_@ntBre reviewed on 2025-03-10 00:03_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:642 on 2025-03-10 00:03_

This is a very minor nit since I don't think this is ever likely to change, but you *could* use `self.kind.changed_version()` for the "added in Python 3.9" part if you wanted to mirror the other `write!` call. I'm happy with the current version too though.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/helpers.rs`:55 on 2025-03-10 08:45_

Nit: The function name suggests that it returns a node where, in fact, it returns a message and a range. 

I think we should either change the method to return a `Option<&Expr>` with a second method that allows deriving a display name for the expression or rename the method

---

_@MichaReiser reviewed on 2025-03-10 08:45_

---

_Comment by @MichaReiser on 2025-03-10 08:45_

@ntBre and I discussed this and we deliberately decided against it because:

* It introduces more complexity and an extra cost for every added syntax
* It seems low priority considering that < Py39 is end of life

I'm not opposed to this change. I just want to make clear that we deliberately decided against it because we wanted to focus our time on higher priority syntax errors and reduce complexity

---

_@dhruvmanila reviewed on 2025-03-10 10:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@decorator_await_expression_py38.py.snap`:94 on 2025-03-10 10:25_

I actually find the "outside function-call arguments in a decorator" part a bit confusing because it made me think that this is only allowed in function-call arguments. I'd possibly remove it and keep it simply "Cannot use `await` expression in a decorator on Python 3.8 ..." but then the message cannot be used in a general way because, in this example, the `await` expression is only not allowed at the top-level (`@(await x)`) while `@decorator(await x)` would be ok.

---

_@MichaReiser approved on 2025-03-10 10:47_

---

_@AlexWaygood reviewed on 2025-03-10 11:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/error.rs`:642 on 2025-03-10 11:46_

hmm, yeah. I did it this way because it seemed like a little less indirection. But your suggestion is probably better because it encodes in exactly one place the version the syntax was added in

---

_@AlexWaygood reviewed on 2025-03-10 13:28_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@decorator_await_expression_py38.py.snap`:94 on 2025-03-10 13:28_

Oh, I see. So you interpreted "outside function-call arguments" as meaning "If I add parentheses, it will be valid syntax"?

Hmm, I'm not sure how to clarify that :/ adding the parentheses _doesn't_ make it a function call -- but I can see how a beginner might find the language confusing. Would you find "outside call expression arguments" any clearer?

---

_Merged by @AlexWaygood on 2025-03-17 11:17_

---

_Closed by @AlexWaygood on 2025-03-17 11:17_

---

_Branch deleted on 2025-03-17 11:17_

---

_@dhruvmanila reviewed on 2025-03-17 11:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@decorator_await_expression_py38.py.snap`:94 on 2025-03-17 11:26_

Sorry for the late reply, what I thought it could be interpreted is related to the function that's being decorated and not the decorator itself. But, I think what you've is fine as well. We can iterate if anyone finds it confusing.

---
