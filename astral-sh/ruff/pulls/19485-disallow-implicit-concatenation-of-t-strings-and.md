```yaml
number: 19485
title: Disallow implicit concatenation of t-strings and other string types
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
  - parser
  - python314
assignees: []
merged: true
base: main
head: t-string-disallow-concat
created_at: 2025-07-22T12:47:46Z
updated_at: 2025-07-27T12:49:51Z
url: https://github.com/astral-sh/ruff/pull/19485
synced_at: 2026-01-12T15:56:40Z
```

# Disallow implicit concatenation of t-strings and other string types

---

_@dylwil3_

As of [this cpython PR](https://github.com/python/cpython/pull/135996), it is not allowed to concatenate t-strings with non-t-strings, implicitly or explicitly. Expressions such as `"foo" t"{bar}"` are now syntax errors.

This PR updates some AST nodes and parsing to reflect this change.

The structural change is that `TStringPart` is no longer needed, since, as in the case of `BytesStringLiteral`, the only possibilities are that we have a single `TString` or a vector of such (representing an implicit concatenation of t-strings). This removes a level of nesting from many AST expressions (which is what all the snapshot changes reflect), and simplifies some logic in the implementation of visitors, for example.

The other change of note is in the parser. When we meet an implicit concatenation of string-like literals, we now count the number of t-string literals. If these do not exhaust the total number of implicitly concatenated pieces, then we emit a syntax error. To recover from this syntax error, we encode any t-string pieces as _invalid_ string literals (which means we flag them as invalid, record their range, and record the value as `""`). Note that if at least one of the pieces is an f-string we prefer to parse the entire string as an f-string; otherwise we parse it as a string.

This logic is exactly the same as how we currently treat `BytesStringLiteral` parsing and error recovery - and carries with it the same pros and cons.

Finally, note that I have not implemented any changes in the implementation of the formatter. As far as I can tell, none are needed. I did change a few of the fixtures so that we are always concatenating t-strings with t-strings.

---

_Label `internal` added by @dylwil3 on 2025-07-22 12:47_

---

_Label `parser` added by @dylwil3 on 2025-07-22 12:47_

---

_Label `python314` added by @dylwil3 on 2025-07-22 12:47_

---

_Comment by @github-actions[bot] on 2025-07-22 12:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-22 13:08_

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

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/nodes.rs`:677 on 2025-07-25 18:04_

Modify this example (since the displayed implicit concatenation is not allowed)

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/nodes.rs`:710 on 2025-07-25 18:05_

Modify this example, since the displayed implicit concatenation is not allowed.

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/comparable.rs`:712 on 2025-07-25 18:07_

```suggestion
    // [CPython implementation] of a `string.templatelib.Template` object.
```

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__parse_tstring.snap`:58 on 2025-07-25 18:12_

This is correct - the actual test has

```rust
        let source = r#"t"{a}{ b }{{foo}}""#;
```

so we are correctly reading the escaped braces.

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__parse_tstring_equals.snap`:45 on 2025-07-25 18:14_

This is also correct - it's just one of those unfortunate diffs that makes it look like something changed when it was just moved.

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/string.rs`:867 on 2025-07-25 18:15_

nit - consistent naming convention

---

_@dylwil3 reviewed on 2025-07-25 18:20_

---

_Marked ready for review by @dylwil3 on 2025-07-25 18:39_

---

_Review requested from @carljm by @dylwil3 on 2025-07-25 18:39_

---

_Review requested from @AlexWaygood by @dylwil3 on 2025-07-25 18:39_

---

_Review requested from @sharkdp by @dylwil3 on 2025-07-25 18:39_

---

_Review requested from @dcreager by @dylwil3 on 2025-07-25 18:39_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-07-25 18:39_

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-07-25 18:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1380 on 2025-07-26 11:21_

I'd prefer using `tstring_count < strings.len()` over a match with an `unreachable`. The idea of using a `match` is to handle all cases but this doesn't seem very useful if one of the cases is impossible anyway

---

_@MichaReiser approved on 2025-07-26 11:26_

---

_@dylwil3 reviewed on 2025-07-27 12:37_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/expression.rs`:1380 on 2025-07-27 12:37_

Here I was mimicking the style used in the similar expression for bytes - so I've changed them both to the form you suggested

---

_Merged by @dylwil3 on 2025-07-27 12:41_

---

_Closed by @dylwil3 on 2025-07-27 12:41_

---
