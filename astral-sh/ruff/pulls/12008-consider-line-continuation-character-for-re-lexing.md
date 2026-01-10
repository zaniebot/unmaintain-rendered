```yaml
number: 12008
title: Consider line continuation character for re-lexing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/re-lexing-newline-escape
created_at: 2024-06-24T07:24:07Z
updated_at: 2024-06-25T02:29:37Z
url: https://github.com/astral-sh/ruff/pull/12008
synced_at: 2026-01-10T21:56:00Z
```

# Consider line continuation character for re-lexing

---

_Pull request opened by @dhruvmanila on 2024-06-24 07:24_

## Summary

This PR fixes a bug where the re-lexing logic didn't consider the line continuation character being present before the newline character. This meant that the lexer was being moved back to the newline character which is actually ignored via `\`.

Considering the following code:
```py
f'middle {'string':\
        'format spec'}

```

The old token stream is:
```
...
Colon 18..19
FStringMiddle 19..29 (flags = F_STRING)
Newline 20..21
Indent 21..29
String 29..42
Rbrace 42..43
...
```

Notice how the ranges are overlapping between the `FStringMiddle` token and the tokens emitted after moving the lexer backwards.

After this fix, the new token stream which is without moving the lexer backwards in this scenario:
```
FStringStart 0..2 (flags = F_STRING)
FStringMiddle 2..9 (flags = F_STRING)
Lbrace 9..10
String 10..18
Colon 18..19
FStringMiddle 19..29 (flags = F_STRING)
FStringEnd 29..30 (flags = F_STRING)
Name 30..36
Name 37..41
Unknown 41..44
Newline 44..45
```

fixes: #12004 

## Test Plan

Add a test case and update the snapshots.

<!-- How was it tested? -->


---

_Label `bug` added by @dhruvmanila on 2024-06-24 07:24_

---

_Label `parser` added by @dhruvmanila on 2024-06-24 07:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-24 07:24_

---

_Renamed from "Consider line continuation char for re-lexing" to "Consider line continuation character for re-lexing" by @dhruvmanila on 2024-06-24 07:24_

---

_Comment by @github-actions[bot] on 2024-06-24 07:43_

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

_@MichaReiser reviewed on 2024-06-24 10:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1387 on 2024-06-24 10:36_

Hmm, it might be more complicated than that... The continuation character could be escaped. But maybe that's not relevant because we already assume that we aren't inside a string?

---

_@dhruvmanila reviewed on 2024-06-24 11:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1387 on 2024-06-24 11:17_

Good point, I'd need to account for the fact that the line continuation character itself can be escaped as well.

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-24 11:54_

---

_@MichaReiser reviewed on 2024-06-24 11:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1393 on 2024-06-24 11:55_

I think it's still more complicate than this. What about `\\\` Here, we have an escaped backslash followed by a continuation :(

---

_@dhruvmanila reviewed on 2024-06-24 11:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1393 on 2024-06-24 11:57_

I see. I guess we'd need to count the number of backslashes and make a decision based on whether it's odd or even.

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-24 12:43_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1391 on 2024-06-24 16:06_

Last comment :) Do we need to restrict the escape handling to cases where we know we're inside a string? Or wouldn't that work in case of an unterminated string literal? 

I'm not sure if it matters because the parser is already in an error recovery state when encountering an escaped `\` outside of a string, but it might be worth to add a test for it

```python
test + a \\\
more
```

---

_@MichaReiser approved on 2024-06-24 16:06_

---

_@dhruvmanila reviewed on 2024-06-25 02:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1391 on 2024-06-25 02:09_

I'm not sure I follow here. Do you mean to ask whether this logic needs to be restricted to only recovering within a string or not? I don't think so that is necessary, I'll add a test case for line continuation character encountered while re-lexing outside of a string.


---

_Merged by @dhruvmanila on 2024-06-25 02:13_

---

_Closed by @dhruvmanila on 2024-06-25 02:13_

---

_Branch deleted on 2024-06-25 02:13_

---
