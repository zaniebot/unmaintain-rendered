```yaml
number: 11563
title: "Update lexer to only emit the `TokenKind`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/token-kind-only
created_at: 2024-05-27T06:27:18Z
updated_at: 2024-05-29T06:09:35Z
url: https://github.com/astral-sh/ruff/pull/11563
synced_at: 2026-01-12T15:55:38Z
```

# Update lexer to only emit the `TokenKind`

---

_@dhruvmanila_

## Summary

This PR updates the lexer to only emit `TokenKind` instead of the `Token` struct.

The token source will construct the `Token` struct right before adding it to the token vector.


---

_Label `internal` added by @dhruvmanila on 2024-05-27 06:27_

---

_@dhruvmanila reviewed on 2024-05-28 09:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:862 on 2024-05-28 09:41_

This is the only reference which we need to keep

---

_@dhruvmanila reviewed on 2024-05-28 09:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token_source.rs`:110 on 2024-05-28 09:42_

There's already a `is_trivia` method on `TokenKind` which also considers indentation and logical newlines. I plan to look into resolving the differences.

---

_Marked ready for review by @dhruvmanila on 2024-05-28 09:42_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-28 09:43_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:811 on 2024-05-28 10:51_

I think we should reset `current_value` to `TokenValue::None` to prevent that a value keeps lingering if it isn't consumed using `take_value`
```suggestion
				self.current_value = TokenValue::None;
        self.current_kind = self.lex_token();
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:862 on 2024-05-28 10:51_

I'm not sure what you mean by that. The only reference to what? 

---

_@MichaReiser approved on 2024-05-28 10:53_

---

_@dhruvmanila reviewed on 2024-05-29 05:51_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:862 on 2024-05-29 05:51_

The only reference to `self.cursor.start_token()`. Earlier, the calls were spread across multiple `lex_*` calls but now it's centralized in the `next_token`. But, this still needs to remain because the lexer might skip whitespaces before this.

---

_Merged by @dhruvmanila on 2024-05-29 06:09_

---

_Closed by @dhruvmanila on 2024-05-29 06:09_

---

_Branch deleted on 2024-05-29 06:09_

---
