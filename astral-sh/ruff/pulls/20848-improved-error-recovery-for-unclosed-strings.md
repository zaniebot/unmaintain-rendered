```yaml
number: 20848
title: Improved error recovery for unclosed strings (including f- and t-strings)
type: pull_request
state: merged
author: MichaReiser
labels:
  - parser
assignees: []
merged: true
base: main
head: micha/unclosed-string
created_at: 2025-10-13T17:15:16Z
updated_at: 2025-10-15T07:50:58Z
url: https://github.com/astral-sh/ruff/pull/20848
synced_at: 2026-01-12T15:57:11Z
```

# Improved error recovery for unclosed strings (including f- and t-strings)

---

_@MichaReiser_

## Summary

This PR improves our lexer to preserve `STRING` tokens instead of converting them to `Unknown` if a string literal misses its closing quotes. 
Instead of converting to `UNKNOWN`, it sets a flag on the string literal that allows upstream tools to check if it's an unclosed string literal.

The benefit of preserving string literals is that it gives us much better error recovery because the parser now recognizes those literals. 
That means, ty will correctly infer the literal type for `a = "unclosed` to be `Literal["unclosed"]`. 

Unfortunately, preserving the kind for unclosed string literals regressed the f-string's and t-string's recovery mechanism. So, I went ahead and improved that too. 

There are a few improvements:

* Preserve the F-STRING middle even if it's unclosed (e.g. `f"unclosed`) instead of parsing this as `f""` 
* Better recovery for missing `}`. E.g., the parser now matches the quotes for `f"{ab"` instead of assuming that the closing quotes start a new string
* Better recovery for `r` format specifiers if the `}` is missing: `f"{ab:r"` now parses the `r` as the raw conversion flag rather than `r"` the start of a raw string literal


Fixes https://github.com/astral-sh/ruff/issues/19751
Fixes https://github.com/astral-sh/ruff/issues/20849


## Review

You probably want to skip the first commit :) It updates all snapshots to now include the `unclosed: <UNCLOSED>`  flag.

## Test Plan

Reviewed and updated the snapshot tests. I also reviewed all usages of `TokenKind::String` to find cases where the missing closing quote could now cause issues. 

This change should have no impact on AST-based lint rules or the formatter because they both only run when there are no parse errors.


---

_Label `parser` added by @MichaReiser on 2025-10-13 17:15_

---

_@MichaReiser reviewed on 2025-10-13 17:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_implicit_str_concat/ISC_syntax_error.py`:8 on 2025-10-13 17:15_

I know this looks silly, but keeping the comment over two lines reduces the snapshot changes.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:732 on 2025-10-13 17:18_

While this increases the size of `TokenFlags`, it doesn't increase the size of `Token`. Which is why I didn't bother with any fancy encoding (e.g. it's unclosed if `RAW_STRING_UPPERCASE` and `RAW_STRING_LOWERCASE` are set)

---

_@MichaReiser reviewed on 2025-10-13 17:18_

---

_Comment by @github-actions[bot] on 2025-10-13 17:26_

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

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lexer.rs`:2877 on 2025-10-13 18:01_

```suggestion
    fn lex_fstring_unclosed() {
```

---

_@AlexWaygood reviewed on 2025-10-13 18:10_

---

_@MichaReiser reviewed on 2025-10-13 18:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:2877 on 2025-10-13 18:20_

Ah come on, my spelling is obviously better

---

_@MichaReiser reviewed on 2025-10-14 09:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:982 on 2025-10-14 09:16_

This error was just wrong. `a = "string<EOF` reported an unexpected string error rather than unclosed string literal

---

_Review requested from @dylwil3 by @MichaReiser on 2025-10-14 09:31_

---

_Marked ready for review by @MichaReiser on 2025-10-14 09:31_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-10-14 09:31_

---

_Renamed from "Better error recovery for unclosed strings (including f- and t-strings)" to "Improved error recovery for unclosed strings (including f- and t-strings)" by @MichaReiser on 2025-10-14 09:33_

---

_Review requested from @ntBre by @MichaReiser on 2025-10-14 16:54_

---

_@ntBre approved on 2025-10-15 03:01_

Nice! This looks great to me!

---

_Merged by @MichaReiser on 2025-10-15 07:50_

---

_Closed by @MichaReiser on 2025-10-15 07:50_

---

_Branch deleted on 2025-10-15 07:50_

---
