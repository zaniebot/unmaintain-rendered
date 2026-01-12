```yaml
number: 14700
title: "[`flake8-annotations`] Fix parsing of \"complex\" forward reference type annotations in `any-type` (ANN401)"
type: pull_request
state: closed
author: sbrugman
labels:
  - parser
assignees: []
base: main
head: fix-parsing-complex-forward-reference-ann401
created_at: 2024-12-01T15:28:59Z
updated_at: 2025-04-28T07:04:01Z
url: https://github.com/astral-sh/ruff/pull/14700
synced_at: 2026-01-12T15:55:48Z
```

# [`flake8-annotations`] Fix parsing of "complex" forward reference type annotations in `any-type` (ANN401)

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #14695.

<!-- What's the purpose of the change? What does it do, and why? -->

The root cause was tricky to find. 
"Complex" forward reference type annotations are handled differently. Forward references are "complex" in this case when the raw contents (without quotes) of the expression with the parsed contents is not contained in the string literal.
https://github.com/astral-sh/ruff/blob/84748be16341b76e073d117329f7f5f4ee2941ad/crates/ruff_python_parser/src/typing.rs#L65

Annotations are recursively parsed, for instance:
`"'int'"` (complex) => `"int"` (simple)

The raw contents of the expression "in\x74" is not contained in the string literal `int` and hence requires multiple complex parsing steps: `"'in\x74'"` (complex) -> `"in\x74"` (complex). This is rare, and probably that's why the bug went unnoticed.

Parsed type annotations are cached by their start offset:
https://github.com/astral-sh/ruff/blob/84748be16341b76e073d117329f7f5f4ee2941ad/crates/ruff_linter/src/checkers/ast/mod.rs#L2544

As it turned out, for these complex annotations the starting offset was not properly updated (removing the quotes), as well already do for the simple annotations. This caused an infinite loop, and eventually a stack overflow.

## Test Plan

<!-- How was it tested? -->

Extended the test suite with regression tests.

---

_Review requested from @MichaReiser by @sbrugman on 2024-12-01 15:29_

---

_Review requested from @dhruvmanila by @sbrugman on 2024-12-01 15:29_

---

_@dylwil3 reviewed on 2024-12-01 20:38_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/typing.rs`:99 on 2024-12-01 20:38_

Does this mean we will still get an overflow for this implicitly concatenated example?

```python
def f(x: "'in''\x84'"): 
    pass
```

---

_@sbrugman reviewed on 2024-12-01 21:31_

---

_Review comment by @sbrugman on `crates/ruff_python_parser/src/typing.rs`:99 on 2024-12-01 21:31_

Yes, this case is not handled yet by this PR.

We can see it's valid syntax:

```python
from typing import get_type_hints


def f(x: "'in''\x74'"): 
    pass


print(get_type_hints(f))   # {'x': <class 'int'>}
```

---

_@sbrugman reviewed on 2024-12-01 21:55_

---

_Review comment by @sbrugman on `crates/ruff_python_parser/src/typing.rs`:99 on 2024-12-01 21:55_

I'll add support for those.

---

_Comment by @github-actions[bot] on 2024-12-02 00:49_

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

_@dhruvmanila reviewed on 2024-12-02 05:01_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/typing.rs`:100 on 2024-12-02 05:01_

Why does this use `closer_len` instead of `opener_len`?

---

_Comment by @dhruvmanila on 2024-12-02 05:02_

Do you think this is an issue that should be solved holistically (https://github.com/astral-sh/ruff/issues/10586)? Because the `relocate_expr` is actually incorrect in the first place.

---

_Comment by @dhruvmanila on 2024-12-02 05:05_

> As it turned out, for these complex annotations the starting offset was not properly updated (removing the quotes), as well already do for the simple annotations.

Sorry, I'm a bit confused still. Why are we using the range without quotes for the parsed expression in case of complex annotation? Because for simple annotations, we just use the range without quotes for the original string expression.

---

_@sbrugman reviewed on 2024-12-02 11:59_

---

_Review comment by @sbrugman on `crates/ruff_python_parser/src/typing.rs`:100 on 2024-12-02 11:59_

Ah, this was `opener_len` before but got lost somewhere in the refactoring.

---

_Label `parser` added by @dylwil3 on 2024-12-02 13:23_

---

_Comment by @dhruvmanila on 2024-12-05 05:43_

I think an ideal solution here would be to fix the `relocate_expr` to transform the entire parsed tree accordingly and then move the `range_exluding_quotes` in `parse_type_annotation` which then passes to both `parse_simple_type_annotation` and `parse_complex_type_annotation`.

---

_@dhruvmanila reviewed on 2024-12-05 05:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/typing.rs`:106 on 2024-12-05 05:46_

The way it would work with simple type annotations is that it first parses the outer string and then the inner string on the next call:
```
parse_simple_type_annotation: "'int'" (10..15)
parse_simple_type_annotation: "int" (11..14)
```

In the case of complex annotations, it seems like the range is always going to be the one without the inner quotes:
```
parse_complex_type_annotation: "'int'" (10..18)
parse_complex_type_annotation: "int" (10..18)
```

---

_Comment by @MichaReiser on 2025-04-28 07:03_

This PR has been stale for quite some time. I'll close it. If someone's interested to work on this, feel free to pick up this PR (taking the feedback into consideration)

As always. Thanks @sbrugman for your work on this

---

_Closed by @MichaReiser on 2025-04-28 07:03_

---
