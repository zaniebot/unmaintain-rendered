```yaml
number: 10372
title: "Remove f-string `UnclosedLbrace` error checking from the lexer"
type: pull_request
state: merged
author: LaBatata101
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: needless-error-checking
created_at: 2024-03-12T20:57:08Z
updated_at: 2025-05-08T21:29:13Z
url: https://github.com/astral-sh/ruff/pull/10372
synced_at: 2026-01-12T15:55:32Z
```

# Remove f-string `UnclosedLbrace` error checking from the lexer

---

_@LaBatata101_

This error check is already handled by the new parser. For some reason the parser gets stuck when parsing f-strings with an unclosed brace, e.g. `f'{'`.

CC: @dhruvmanila 


---

_Review requested from @MichaReiser by @LaBatata101 on 2024-03-12 20:57_

---

_Review requested from @dhruvmanila by @zanieb on 2024-03-12 21:12_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/expressions/unclosed_fstring_lbrace.py`:5 on 2024-03-13 06:58_

Can you tell me why are these commented out? I guess it's because the parser is panicking otherwise? If so, then even the first one should be commented out for now, right? We can add a `TODO` comment on top to fix these.

---

_@dhruvmanila reviewed on 2024-03-13 06:59_

---

_@dhruvmanila approved on 2024-03-13 07:05_

Thanks, I agree that this is now redundant.

> For some reason the parser gets stuck when parsing f-strings with an unclosed brace, e.g. `f'{'`.

It's because of the same problem as I mentioned on Discord. The lexer is emitting two errors on the same location - unexpected end of file and unterminated f-string. The former is coming from `consume_end` function. while the latter is occurring because the f-string is still on the stack. We try to parse a f-string middle token but we've already reached end of file, so the lexer thinks it's an unterminated f-string.

I think the solution for this would be to not emit the unexpected EOF error as you mentioned but another solution, which is specific to f-string lexing, would be to not emit the unterminated f-string error if it encountered an EOF token and the nesting level is > 0.



---

_Label `parser` added by @dhruvmanila on 2024-03-13 07:05_

---

_@LaBatata101 reviewed on 2024-03-13 12:17_

---

_Review comment by @LaBatata101 on `crates/ruff_python_parser/resources/invalid/expressions/unclosed_fstring_lbrace.py`:5 on 2024-03-13 12:17_

Oh, I forgot to uncomment 😅

---

_@dhruvmanila reviewed on 2024-03-13 14:04_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/invalid/expressions/unclosed_fstring_lbrace.py`:5 on 2024-03-13 14:04_

Do you expect them to pass with the current state of the parser? I think it'll panic considering that there will be two `Unknown` tokens emitted corresponding to the two errors and it will seem that the parser isn't progressing.

I'm fine with merging with a TODO comment to fix them.

---

_@LaBatata101 reviewed on 2024-03-13 19:37_

---

_Review comment by @LaBatata101 on `crates/ruff_python_parser/resources/invalid/expressions/unclosed_fstring_lbrace.py`:5 on 2024-03-13 19:37_

> Do you expect them to pass with the current state of the parser?

No, it will still get stuck because of the EOF error returned by the lexer, but if we remove the line that returns that error from the lexer, then it works. 

---

_Comment by @LaBatata101 on 2024-03-13 19:41_

> I think the solution for this would be to not emit the unexpected EOF error as you mentioned but another solution, which is specific to f-string lexing, would be to not emit the unterminated f-string error if it encountered an EOF token and the nesting level is > 0.

It's better not to emit the unexpected EOF error since this error is redundant now with the new parser.

---

_Merged by @dhruvmanila on 2024-03-14 03:26_

---

_Closed by @dhruvmanila on 2024-03-14 03:26_

---

_Comment by @github-actions[bot] on 2024-03-14 03:35_

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

_Branch deleted on 2025-05-08 21:29_

---
