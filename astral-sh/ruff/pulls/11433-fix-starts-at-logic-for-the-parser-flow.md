```yaml
number: 11433
title: Fix starts at logic for the parser flow
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/lazy-lexer
head: dhruv/starts-at
created_at: 2024-05-15T11:28:16Z
updated_at: 2024-05-27T08:46:00Z
url: https://github.com/astral-sh/ruff/pull/11433
synced_at: 2026-01-12T15:55:38Z
```

# Fix starts at logic for the parser flow

---

_@dhruvmanila_

## Summary

This PR fixes the parser flow for the starts at functionality.

The parser APIs are as follows:
* `Parser::new`
* `Parser::new_starts_at`

The `TokenSource` and `Lexer` will have the same function signature where they also accepts a start offset. This makes the APIs easier to reason about and avoids adding both variants for the token source and the lexer.

The `*_starts_at` APIs now expect the entire source code and the lexer will skip that many bytes as the offset. This will match the `*_starts_at` naming as well.

The high level APIs which are part of the public interface will be updated to take in a range instead of the offset. Internally, we'll slice the source code up to the end range and pass this sub-string and the start range value to the parser. This change will be done at the end as it affects all the downstream calls as well.

So, all in all, the final APIs will be:
```rs
pub fn parse(source: &str, mode: Mode) -> Program {
	Parser::new(source, mode).parse_program()
}

pub fn parse_range(source: &str, mode: Mode, range: TextRange) -> Program {
	let source = source[..range.end().to_usize()];
	Parser::new_starts_at(source, mode, range.start()).parse_program()
}
```

Both the above APIs expect the entire source code. This is so that the caller doesn't need to worry about slicing the correct part of the source code.


---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-15 11:28_

---

_Label `parser` added by @dhruvmanila on 2024-05-15 11:28_

---

_Merged by @dhruvmanila on 2024-05-15 11:28_

---

_Closed by @dhruvmanila on 2024-05-15 11:28_

---

_Branch deleted on 2024-05-15 11:28_

---

_@MichaReiser reviewed on 2024-05-27 08:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:113 on 2024-05-27 08:46_

Should this be set to `start_offset` for the `if self.last_token_end <= start` check in `node_range` to work as expected?

---
