```yaml
number: 21310
title: "[`flake8-logging-format`] Fix G004 preview mode breaking on f-strings with control characters (`G004`)"
type: pull_request
state: open
author: danparizher
labels: []
assignees: []
base: main
head: fix-21295
created_at: 2025-11-07T04:35:43Z
updated_at: 2025-11-10T22:25:47Z
url: https://github.com/astral-sh/ruff/pull/21310
synced_at: 2026-01-10T16:53:55Z
```

# [`flake8-logging-format`] Fix G004 preview mode breaking on f-strings with control characters (`G004`)

---

_Pull request opened by @danparizher on 2025-11-07 04:35_

## Summary

Fixes #21295. The G004 rule's preview mode fix was generating invalid Python syntax when converting f-strings containing control characters (newlines, tabs, carriage returns) to `%` formatting in logging calls. This fix adds proper escaping of control characters and quotes when building the replacement string literal.

## Problem Analysis

When G004's preview mode converts f-strings to `%` formatting, it directly appends literal text from the f-string parts to the format string without escaping control characters. This causes syntax errors when the generated code contains unescaped newlines, tabs, or other control characters in string literals.

The root cause was in the `logging_f_string` function in `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`, where literal text from f-string parts was appended directly to the format string without escaping control characters (lines 60 and 72 in the original code).

## Approach

1. **Added escape helper function**: Created `escape_string_for_literal()` that properly escapes control characters (`\n`, `\t`, `\r`), backslashes (`\`), and quotes matching the quote style for use in Python string literals.

2. **Updated literal handling**: Modified the `logging_f_string` function to use the escape function when appending literal text from both:
   - `FStringPart::Literal` parts (line 83)
   - `InterpolatedStringElement::Literal` elements (lines 95-98)

3. **Added test cases**: Added test cases covering newlines, tabs, and carriage returns to ensure the fix works correctly and prevents regressions.

The fix ensures that all control characters are properly escaped in the generated Python code, making it syntactically valid while preserving the intended behavior of the original f-string.


---

_Comment by @github-actions[bot] on 2025-11-07 04:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-11-10 22:25_

Hmm, I'm not sure this is the right approach. There's another open PR and issue related to this rule in https://github.com/astral-sh/ruff/pull/20224 and https://github.com/astral-sh/ruff/issues/20151, respectively. Many of the issues here seem to be that we're performing unnecessary string manipulations on the contents of the f-string when it would be better only to very narrowly replace the interpolations with `%s`.

Actually now that I look at it again, I think #21295 is a duplicate of #20151 too. If @TaKO8Ki plans to get back to the other PR, it might be better to close this in favor of that one (or vice versa if not). Sorry for the confusion!

---
