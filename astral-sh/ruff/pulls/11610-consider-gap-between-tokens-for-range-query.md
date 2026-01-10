```yaml
number: 11610
title: "Consider \"gap\" between tokens for range query"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/tokens-range-query
created_at: 2024-05-30T05:54:38Z
updated_at: 2024-05-30T10:05:04Z
url: https://github.com/astral-sh/ruff/pull/11610
synced_at: 2026-01-10T21:56:00Z
```

# Consider "gap" between tokens for range query

---

_Pull request opened by @dhruvmanila on 2024-05-30 05:54_

## Summary

This PR updates the methods on `Tokens` struct to consider the "gap" between tokens. Additionally, it adds a constraint to the methods which is that the offsets shouldn't be within the token range otherwise it'll panic.

## Test Plan

Add unit test cases.


---

_Label `parser` added by @dhruvmanila on 2024-05-30 05:54_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-30 05:54_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-30 06:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:474 on 2024-05-30 06:55_

I think the error message here is incorrect. The offset can be between two tokens (it can be inside the gap), but it shouldn't be inside a token. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:445 on 2024-05-30 06:56_

I think the error message here is incorrect. The end offset can be in between two tokens (it can fall into a gap), but it should never be inside a token.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:470 on 2024-05-30 07:10_

I think `saturating_sub` here is incorrect when the file contains a BOM header, in which case the first token starts at offset 1 (or 2?). 

For `offset == 0`:

* `start == 0` 
* `token.start() == 1` (and, therefore, `offset != token.start()`
* `saturating_sub` returns `0` (again the first token)
* `offset` < `token.end()` and panic, where it shouldn't. I think we need to manually test here that `start > 0 && self.get(start - 1).is_some_and...

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:478 on 2024-05-30 07:12_

I think you could use `binary_search_by` here (and probably above as well). It already makes the distinction for you whether it found an element that precisely started at `offset` (and `partition_point` calls into `binary_search_by`)

```rust
        match self.binary_search_by(|token| token.start().cmp(&offset)) {
            Ok(idx) => &self[idx..],
            Err(idx) => {
                // Test that the offset doesn't fall inside the last token that started before offset.
                if idx > 0 && self.get(idx - 1).is_some_and(|token| token.end() > offset) {
                    panic!("Offset cannot be in between a token")
                }

                // Offset falls between two tokens
                &self[idx..]
            }
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:627 on 2024-05-30 07:14_

Nit: I would consider splitting the tests into smaller tests with more specific names. E.g. `tokens_after_offset_at_token_start`, `tokens_after_offset_inside_token`, `tokens_after_offset_at_token_end`, `tokens_after_offset_between_tokens`

I also recommend to remove the `test` prefix from test methods. It gives you nicer test names when running the tests.

---

_@MichaReiser approved on 2024-05-30 07:14_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-05-30 07:41_

---

_@dhruvmanila reviewed on 2024-05-30 08:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:478 on 2024-05-30 08:44_

Oh nice, thanks!

---

_@dhruvmanila reviewed on 2024-05-30 08:59_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:470 on 2024-05-30 08:59_

Good catch. I just checked, it starts at offset 3:
```
3 Name {
    name: "hello",
} 8
8 Newline 8
```

---

_Merged by @dhruvmanila on 2024-05-30 10:05_

---

_Closed by @dhruvmanila on 2024-05-30 10:05_

---

_Branch deleted on 2024-05-30 10:05_

---
