```yaml
number: 10339
title: Merge string parsing to single entrypoint
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/string-parsing
created_at: 2024-03-11T13:25:38Z
updated_at: 2024-03-12T08:48:05Z
url: https://github.com/astral-sh/ruff/pull/10339
synced_at: 2026-01-12T15:55:31Z
```

# Merge string parsing to single entrypoint

---

_@dhruvmanila_

## Summary

This PR merges the different string parsing functions into a single entry point function.

Previously there were two entry points, one for string or byte literal and the other for f-strings. The reason for this separation is that our old parser raised a hard syntax error if an f-string was used as a pattern literal. But, it's actually a soft syntax error as evident through the CPython parser which raises it during the runtime.

This function basically implements the following grammar:
```
strings: (string|fstring)+
```

And it delegates it to the list parsing for better error recovery.

## Test Plan

- [x] All tests pass
- [x] No performance regression


---

_Label `parser` added by @dhruvmanila on 2024-03-11 13:25_

---

_@dhruvmanila reviewed on 2024-03-12 08:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:388 on 2024-03-12 08:34_

Grammar: `strings: (string|fstring)+`

---

_@dhruvmanila reviewed on 2024-03-12 08:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:796 on 2024-03-12 08:36_

I've seen this kind of match statement being written as:
```rs
if parser.at(TokenKind::String) {
	// ...
} else if parser.at(TokenKind::FStringStart) {
	// ...
}
```

Which then avoids having an `unreachable` branch. I might switch to that.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:836 on 2024-03-12 08:39_

This was valid before the refactor as well. The match statement was only for a single string and _not_ an implicitly concatenated one.

---

_@dhruvmanila reviewed on 2024-03-12 08:39_

---

_Marked ready for review by @dhruvmanila on 2024-03-12 08:45_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-12 08:45_

---

_Merged by @dhruvmanila on 2024-03-12 08:48_

---

_Closed by @dhruvmanila on 2024-03-12 08:48_

---

_Branch deleted on 2024-03-12 08:48_

---
