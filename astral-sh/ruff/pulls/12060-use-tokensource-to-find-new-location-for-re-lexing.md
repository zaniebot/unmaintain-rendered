```yaml
number: 12060
title: "Use `TokenSource` to find new location for re-lexing"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/re-lexing
created_at: 2024-06-27T04:11:20Z
updated_at: 2024-06-27T11:42:41Z
url: https://github.com/astral-sh/ruff/pull/12060
synced_at: 2026-01-10T21:56:00Z
```

# Use `TokenSource` to find new location for re-lexing

---

_Pull request opened by @dhruvmanila on 2024-06-27 04:11_

## Summary

This PR splits the re-lexing logic into two parts:
1. `TokenSource`: The token source will be responsible to find the position the lexer needs to be moved to
2. `Lexer`: The lexer will be responsible to reduce the nesting level and move itself to the new position if recovered from a parenthesized context

This split makes it easy to find the new lexer position without needing to implement the backwards lexing logic again which would need to handle cases involving:
* Different kinds of newlines
* Line continuation character(s)
* Comments
* Whitespaces

### F-strings

This change did reveal one thing about re-lexing f-strings. Consider the following example:
```py
f'{'
#  ^
f'foo'
```

Here, the quote as highlighted by the caret (`^`) is the start of a string inside an f-string expression. This is unterminated string which means the token emitted is actually `Unknown`. The parser tries to recover from it but there's no newline token in the vector so the new logic doesn't recover from it. The previous logic does recover because it's looking at the raw characters instead.

The parser would be at `FStringStart` (the one for the second line) when it calls into the re-lexing logic to recover from an unterminated f-string on the first line. So, moving backwards the first character encountered is a newline character but the first token encountered is an `Unknown` token.

This is improved with #12067 

fixes: #12046 
fixes: #12036

## Test Plan

Update the snapshot and validate the changes.


---

_Comment by @github-actions[bot] on 2024-06-27 04:50_

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

_Label `bug` added by @dhruvmanila on 2024-06-27 09:51_

---

_Label `parser` added by @dhruvmanila on 2024-06-27 09:51_

---

_Marked ready for review by @dhruvmanila on 2024-06-27 10:20_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-27 10:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1386 on 2024-06-27 11:22_

Oh wow, that's a lot of code that is gone now :)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1413 on 2024-06-27 11:23_

Does it still make sense to reduce the nesting level above unconditionally or could we invert the condition here and only then reduce the nesting?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token_source.rs`:80 on 2024-06-27 11:25_

Not important and I'm fine to keep it this way. I was just wondering if we could store the offset of the `non_logical_newline` and truncate the `self.tokens` to that position if `re_lex_logical_token` returns `true`.

---

_@MichaReiser approved on 2024-06-27 11:25_

Nice!

---

_@dhruvmanila reviewed on 2024-06-27 11:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1413 on 2024-06-27 11:34_

Yeah, good point, we can invert the condition

---

_@dhruvmanila reviewed on 2024-06-27 11:35_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1413 on 2024-06-27 11:35_

Hmm, actually, it might still not be possible. Let me confirm

---

_Merged by @dhruvmanila on 2024-06-27 11:42_

---

_Closed by @dhruvmanila on 2024-06-27 11:42_

---

_Branch deleted on 2024-06-27 11:42_

---
